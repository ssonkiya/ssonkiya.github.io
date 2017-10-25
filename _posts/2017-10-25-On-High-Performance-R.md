A few weeks ago I put a call out to Rstats twitter: 

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr"><a href="https://twitter.com/hashtag/rstats?src=hash&amp;ref_src=twsrc%5Etfw">#rstats</a> twitter - who loves helping to make (short) code run as fast as possible? Playing w/ foreach, doparallel, data.table but know little</p>&mdash; Emily Robinson (@robinson_es) <a href="https://twitter.com/robinson_es/status/915632978524540928?ref_src=twsrc%5Etfw">October 4, 2017</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

I had a working, short script that took 30 seconds to run. But while this may be fine if you only need to run it once, but I needed to run it hundreds of time for simulations. My first attempt to do so ended about four hours after I started the code, with 400 simulations left to code, and I knew I needed to get some help.  

## The Problem 

At Etsy I work a lot on our A/B Testing. When assigning browsers randomly to experimental groups, we do so based on their browser id (cookie) or device id for apps. But when we analyze our data, we can use two different methods: visits and browser level. Visit-level means we break up browsers into chunks of behavior that don't have more than 30 minutes of inactivity between events. For more details, see more talk on [A/B Testing](tiny.cc/abtalk) (starting at 17:30).

We analysts encourage everyone to favor browser metrics over visit metrics. One reason is that visits are not independent, as multiple visits can come from the same browser or person. This violates the independence assumption of the tests we use, which should inflate our false positive rate. We'd never actually tested this with our own browsers, however, and this theoretical concept was not always convincing to our partner teams. 

Therefore, I set out to simulate hundreds of null A/B Tests using our own data. I wanted to take all the visits who saw a certain page, like search, assign them randomly to A or B, and use a proportion test for the conversion rate (percentage of visits that bought). Looking at the p-values of hundreds of these tests, I would see if the percentage with p < .05, our false positive rate, was actually around 5% or if it was inflated, as we expected. 

## Performance Optimization

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">One rule of thumb for maximizing <a href="https://twitter.com/hashtag/rstats?src=hash&amp;ref_src=twsrc%5Etfw">#rstats</a> performance is that the way you&#39;d do something once is rarely the best way to do it 1000X</p>&mdash; David Robinson (@drob) <a href="https://twitter.com/drob/status/915987148515377152?ref_src=twsrc%5Etfw">October 5, 2017</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

At RStudio::Conf 2017, Hadley Wickham [discussed](https://robinsones.github.io/RStudio-Conference-Tips-and-Tricks/) how the bottleneck in writing code is usually thinking speed, not computational speed, and so you shouldn't prematurely worry about optimizing for performance but about making sure your code is clear. This holds true for the majority of my code, which I only need to run once. In this case, though, the point was to run the same code hundreds of times. 

## Asking for Help 

I am very fortunate to have a data scientist brother who will jump in to help out even when not directly asked:  

![center](https://github.com/robinsones/robinsones.github.io/blob/master/images/Dave_optimization_tweet.png)

I've also gotten to know many other people in the R community through RLadies, conferences, or twitter, some of whom offered help as well. But even if you're new to R and don't know anyone, there are people who will jump in and help you too. RStudio recently launched a new [community website](https://community.rstudio.com/) where you can ask quesitons ranging from a specific issue that you need to debug to why you should use RMarkdown.  

Part of why I wrote this post is I believe those who are privileged--whether by having a data science job, getting to go to conferences, or --should try to share that through public work. 
## Lessons Learned

#### You may not need big data tools

In my tweet, I mentioned some packages I'd been trying, including foreach and doparallel, packages for parallel processing. Some folks replying also suggested sparklyr (integrates spark and R) and Rcpp (integrates R and C++). I thought I needed to use these because I was dealing with big data - 10+ million rows with some text columns! 

But even if the data you start with is large, you may be able to make it smaller by summarizing or eliminating extra columns. In the next section, you'll see how I compressed my data into 3 numeric columns and less than a thousand rows. 

#### Try to do everything (grouping and counting) you can in SQL and eliminate unnecessary information

Here was my original code that:

1. Pulled a table down from SQL that has all the visits that had a search in the past week, including whether they converted or not, and their browser id
2. Randomly assigned on the **browser level** a label of 0 or 1
3. Counted up the number of total visits and converting visits for each label. 
4. Ran a prop test comparing the two groups. 

``` sql
SELECT * FROM erobinson.simulate_fp_search
```

``` r
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
```

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

``` sql
SELECT count(*) as total_visits, sum(converted) as converted 
FROM erobinson.simulate_fp_search
GROUP BY browser_id 
```

``` r
simulate_p_value_visits <- function() {
  results <- search_visits %>%
    # group_by here is really a mutate and group_by
    group_by(label = sample(c(0,1), n(), replace = TRUE)) %>%
    summarize(total_visits = sum(total_visits), converted = sum(converted)) 
  
  prop.test(results$converted, results$total_visits)$p.value
}

simulate_p_values_visit_result <- replicate(3000, simulate_p_value_visits())

```

While switching the dplyr to data.table could probably speed it up even more, right now it's running pretty quickly. 

### Eliminate Redundancy 

But we can then recognize that our table currently has a lot of redudancy: we have many browsers that have 1 visits and 0 conversions, 2 visits and 0 conversion, etc. Therefore, we can make our code faster by: 
1. Transform our table so each row is a unique combination of visits & conversions, with a column that is the number of browsers with that combination. We can do this in SQL and output the table as "count_of_counts".
2. Use the binomial distribution to simulate splitting browsers into A and B groups. 
3. As before, summarize the number of visits and conversions in each group and apply our proportion test.

``` sql
with counts as (
  SELECT count(*) as total_visits, sum(converted) as converted 
  FROM erobinson.simulate_fp_search
  GROUP BY browser_id
)
  SELECT count(*) as n, total_visits, converted
  FROM browsers_summarized
  GROUP BY total_visits, converted
```

``` r
simulate_p_value <- function() {
  # put about half (with binomial sampling) in each group
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
```

### Vectorize

While the previous code is pretty fast, we can get it even faster by vectorizing the prop test. If you're not familiar with vectorization in R and why it's faster, check out Noam Ross's [excellent blog post](http://www.noamross.net/blog/2014/4/16/vectorization-in-r--why.html). 

Here is the new code using a vectorized proportion test (courtsey of David Robinson's [splittestr package](https://github.com/dgrtwo/splittestr). 

``` r
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
```

Crossing is the tidyr version of mutate: it creates a tibble from all the combinations of the supplied vectors. In this case, that means we'll have a 1000x the number of rows in count_of_counts. For each of them, we'll simulate putting half of the browsers in A and half in B. Then we can get the total number of visits and converted visits for each trial and use our vectorized prop test, creating a new variable that is the p-value. 

### Use Matrix Operations

But wait, there's more! The issue with the previous version is memory: we're creating that intermediate product that has hundreds of thousands of rows. Instead, we can use matrix operations, which are faster and less memory-taxing than R. We don't get to use tidyverse code, but sometimes sacrifices must be made. 

``` r
A <- replicate(1000, rbinom(nrow(count_of_counts), count_of_counts$n, .5))
B <- count_of_counts$n - A

total_A <- colSums(A * count_of_counts$total)
total_B <- colSums(B * count_of_counts$total)
converted_A <- colSums(A * count_of_counts$converted)
converted_B <- colSums(B * count_of_counts$converted)

pvals <- vectorized_prop_test(converted_A, total_A - converted_A,
                              converted_B, total_B - converted_B)
                              
false_positive_rate <- sum(pvals < .05)/length(pvals)*100                         
```

Here, we first create two matrixes, A and B. Their columns are each one simulation, and the rows are each one combination of visits and conversion (e.g. one is for browsers with 1 visit and 0 conversion, another for 2 visits and 1 conversion, etc). We've used the binomial distribution again to simulate splitting n, the number of browsers with a combination of visits and conversions, into A and B.

We create four vectors of length 1000, one entry for each simulation. Two are for the total number of visits in A or B and two are for the the number of converted visits in A or B. For example, if the first entry in A and B represents the number of browsers with 5 visits and 1 conversions in A and B, respectively, we just multiply each of those by 5 to get the number of visits and by 1 to get the number of conversion. So if it was 2 in A and 3 in B, that means A had 10 visits and 2 conversion while B had 15 visits and 3 conversions. We do this for every row and then add up the column to get the total number of visits and total number of conversions for that simulation in A and in B. This operation is repeated for all 1000 columns (simulations). 

Our last step is to use the vectorized prop test to get a 1000 p-values and then calculate what percentage of them are less than .05

### Take Home Lesson

