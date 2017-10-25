A few weeks ago I put a call out to Rstats twitter: 

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr"><a href="https://twitter.com/hashtag/rstats?src=hash&amp;ref_src=twsrc%5Etfw">#rstats</a> twitter - who loves helping to make (short) code run as fast as possible? Playing w/ foreach, doparallel, data.table but know little</p>&mdash; Emily Robinson (@robinson_es) <a href="https://twitter.com/robinson_es/status/915632978524540928?ref_src=twsrc%5Etfw">October 4, 2017</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

I had an working, short script that took 30 seconds to run. But while this may be fine if you only need to run it once, but I needed to run it hundreds of time for simulations. 

## The Problem 

I work on experimentation at Etsy. We assign browsers to experiments based on their browser id (cookie) or device id for apps. We analyze our data in two different ways though: visits and browser level. Visit level means we break up browsers into chunks of behavior not interrupted by more than 30 minutes of inactivity. For more details, see more talk starting at XX time.

## Lessons Learned

#### Try to do everything (grouping and counting) you can in SQL

Here was my original code for:
1. Pulling a table down from SQL that has all the visits that had a search in the past week, including whether they converted or not, and their browser id
2. Randomly assigning on the **browser level** a label of 0 or 1
3. Counting up the number of total visits and converting visits for each label. 

{% highlight sql %}
SELECT * FROM erobinson.simulate_fp_search
{% endhighlight %}

{% highlight r %}
N <- nrow(distinct(search_visits, browser_id)

browsers <- search_visits %>%
  distinct(browser_id) %>%
  mutate(ab_variant = sample(c(0,1), N, replace = TRUE))

browsers <- as.data.table(browsers)
browsers <- setkey(browsers, browser_id)
dat_w_labels <- merge(search_visits, browsers, all.x=TRUE)
dat_w_labels <- dat_w_labels[, .(.N), by = .(ab_variant, converted)] %>% arrange(ab_variant)

failures <- dat_w_labels %>%
  filter(converted == 0) %>% 
  pull(N)

successes <-  dat_w_labels %>%
  filter(converted == 1) %>% 
  pull(N)

res <- prop.test(successes, failures + successes)
{% endhighlight %}

This table was over 10 million rows, and I was doing many operations:
1. Getting the number of distinct browser_ids
2. Making a new table that had every browser_id and a label for them 
3. Merging the new table with the original search visits table
4. Grouping that new table by whether the visit converted or not and it's label, counting the number of each type
5. Extracting the number of conversions and non-conversions 
6. Doing a prop test

The last three steps are fast, but the first three are very long. 

We're able to refactor and make it faster by realizing a few things: 
1. We don't need the big text columns visit_id and browser_id
2. In SQL, we can group by browser_id so we have a row with 1) the number of visis by a browser and 2) the number of visits that converted. With that, we'll have a smaller table of only two numeric columns. 
3. We can then label each row with 0 or 1 randomly, assigning treatment on the browser level. 
4. Next, we sum up the total_visits column and the converted column, grouping by label. 
5. Finally, we run a prop.test

Here's what the new code looks like: 

```sql
SELECT count(*) as total_visits, sum(converted) as converted 
FROM erobinson.simulate_fp_search
GROUP BY browser_id 
```

{% highlight r %}
simulate_p_value_visits <- function() {
  results <- search_visits %>%
    # group_by here is really a mutate and group_by
    group_by(label = sample(c(0,1), n(), replace = TRUE)) %>%
    summarize(total_visits = sum(total_visits), converted = sum(converted)) 
  
  prop.test(results$converted, results$total_visits)$p.value
}

simulate_p_values_visit_result <- replicate(3000, simulate_p_value_visits())

{% endhighlight %}

While switching the dplyr to data.table could probably speed it up even more, right now it's running pretty quickly. 

But we can then recognize that our table currently has a lot of redudancy: we have many browsers that have 1 visits and 0 conversions, 2 visits and 0 conversion, etc. Therefore, we can make our code faster by: 
1. Transform our table so each row is a unique combination of visits & conversions, with a column that is the number of browsers with that combination. 
2. Use the binomial distribution to simulate splitting browsers into A and B groups. 
3. As before, summarize the number of visitis and conversions in each group and apply our proportion test.

{% highlight r %}
count_of_counts <- count_converted_by_browser %>%
  count(total, converted)

simulate_p_value <- function() {
  # put about half (with binomial sampling) of each group
  result <- count_of_counts %>%
    mutate(A = rbinom(n(), n, .5),
           B = n - A) %>%
    summarize(total_A = sum(total * A),
              total_B = sum(total * B),
              converted_A = sum(converted * A),
              converted_B = sum(converted * B))

  prop.test(c(result$converted_A, result$converted_B), c(result$total_A, result$total_B))$p.value
}

sim_pvals <- replicate(1000, simulate_p_value())
{% endhighlight %}


This speeds it up a lot, but we can get even faster by vectorizing the prop test. 

{% highlight r %}
vectorized_prop_test <- function(a, b, c, d) {
  n1 <- a + b
  n2 <- c + d
  n <- n1 + n2
  p <- (a + c) / n
  E <- cbind(p * n1, (1 - p) * n1, p * n2, (1 - p) * n2)

  x <- cbind(a, b, c, d)

  DELTA <- a / n1 - c / n2
  YATES <- pmin(.5, abs(DELTA) / sum(1 / n1 + 1 / n2))

  STATISTIC <- rowSums((abs(x - E) - YATES)^2 / E)
  PVAL <- pchisq(STATISTIC, 1, lower.tail = FALSE)
  PVAL
}

count_of_counts <- count_converted_by_browser %>%
  count(total, converted)

simulated_pvals <- count_of_counts %>%
  crossing(trial = 1:1000) %>%
  mutate(A = rbinom(n(), n, .5), B = n - A) %>%
  group_by(trial) %>%
  summarize(total_A = sum(total * A),
            total_B = sum(total * B),
            converted_A = sum(converted * A),
            converted_B = sum(converted * B)) %>%
  mutate(pvalue = vectorized_prop_test(converted_A, total_A - converted_A, converted_B, total_B - converted_B))
{% endhighlight %}

Crossing is the tidyr version of mutate: it creates a tibble from all the combinations of the supplied vectors. In this case, that means we'll have a 1000x the number of rows in count_of_counts. For each of them, we'll simulate putting half of the browsers in A and half in B. Then we can get the total number of visits and converted visits for each trial and use our vectorized prop test, creating a new variable that is the p-value. 

But wait, there's more! The issue with the previous version is memory: we're creating that intermediate product that has hundreds of thousands of rows. Instead, we can use matrix operations, which are faster and less memory-taxing than R. We don't get to use tidyverse code, but sometimes sacrifices must be made. 

{% highlight r %}
count_of_counts <- count_converted_by_browser %>%
  count(total_visits, converted)

A <- replicate(1000, rbinom(nrow(count_of_counts), count_of_counts$n, .5))
B <- count_of_counts$n - A

total_A <- colSums(A * count_of_counts$total)
total_B <- colSums(B * count_of_counts$total)
converted_A <- colSums(A * count_of_counts$converted)
converted_B <- colSums(B * count_of_counts$converted)

pvals <- vectorized_prop_test(converted_A, total_A - converted_A,
                              converted_B, total_B - converted_B)
{% endhighlight %}
