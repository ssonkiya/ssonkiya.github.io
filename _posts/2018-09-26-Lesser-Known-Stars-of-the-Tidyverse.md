---
title: "Lesser Known Stars of the Tidyverse"
author: "Emily Robinson"
date: "2018-09-26"
---

In early 2018, I gave a few conference talks on "The Lesser Known Stars of the Tidyverse." I focused on some packages and functions that aren't as well known as the core parts of ggplot2 and dplyr but are very helpful in exploratory analysis. I walked through an example analysis of [Kaggle's 2017 State of Data Science and Machine Learning Survey](https://www.kaggle.com/surveys/2017) to show how I would use these functions in an exploratory analysis. 

This post shares that analysis with some extra written commentary. If you'd like to watch the talks as well, both the [RStudio::conf](https://www.rstudio.com/resources/videos/the-lesser-known-stars-of-the-tidyverse/) and [New York R conference](https://www.youtube.com/watch?v=ax4LXQ5t38k) ones were recorded. HAPPY!

## Reading in the data



First, we'll try reading in our dataset with base R's `read.csv`.


{% highlight r %}
multiple_choice_responses_base <- read.csv("multipleChoiceResponses.csv")
{% endhighlight %}

Let's say we wanted to know the numbers of NAs in each column. We can use `is.na` to change each entry in a column to TRUE or FALSE, depending on whether it's NA, and then sum the column (because `TRUE` evaluates as 1 and `FALSE` as 0) to get the total number of NAs. 


{% highlight r %}
# for one column
sum(is.na(multiple_choice_responses_base$Country))
{% endhighlight %}



{% highlight text %}
## [1] 0
{% endhighlight %}

To do this for every column, we can use `:map_df` from the package `purrr`, which applies the given function over the whole dataset, column by column, and returns a dataframe with the same column names and one row representing the number of NAs in each column. If you're used to the `apply` family of functions, `purrr` offers the same capabilities in a more consistent way. The `_df` part of the function represents what we want back (a dataframe); using `map` would get us a list. 

To avoid printing out all 228 column names, let's select just a few columns. `select` can take numbers, representing the position of the columns, as well as column names, so we can enter `1:5` to get the first five columns.


{% highlight r %}
# for every column 
multiple_choice_responses_base %>%
  select(1:5) %>%
  map_df(~sum(is.na(.))) 
{% endhighlight %}



{% highlight text %}
## # A tibble: 1 x 5
##   GenderSelect Country   Age EmploymentStatus StudentStatus
##          <int>   <int> <int>            <int>         <int>
## 1            0       0   331                0             0
{% endhighlight %}

Wow that's lucky! Four of them that don't have any NAs. But ... is this too good to be true? Let's look at the entries of one column.  


{% highlight r %}
multiple_choice_responses_base %>%
  count(StudentStatus) 
{% endhighlight %}



{% highlight text %}
## # A tibble: 3 x 2
##   StudentStatus     n
##   <fct>         <int>
## 1 ""            15436
## 2 No              299
## 3 Yes             981
{% endhighlight %}

Yep. We see here we have a lot of `""` entries instead of NAs. We can correct this with `na_if` from `dplyr`, which takes as an argument what we want to turn into NAs. We can also use `%<>%`, which is a reassignment pipe. While this is nice to save some typing, it can make it confusing when reading a script, so use with caution. 


{% highlight r %}
multiple_choice_responses_base %<>%
  na_if("")

## is the same as: 

multiple_choice_responses_base <- multiple_choice_responses_base %>%
  na_if("")
{% endhighlight %}

Now let's count the NAs again. 


{% highlight r %}
multiple_choice_responses_base %>%
  map_df(~sum(is.na(.))) %>%
  select(1:5)
{% endhighlight %}



{% highlight text %}
## # A tibble: 1 x 5
##   GenderSelect Country   Age EmploymentStatus StudentStatus
##          <int>   <int> <int>            <int>         <int>
## 1           95     121   331                0         15436
{% endhighlight %}

And it's fixed! 

How could we have avoided this all in the first place? By using `readr::read_csv` instead of `read.csv`.

If you're not familiar with `::`, it's for explicitly setting what package you're getting the function on the right from. This is helpful in three ways:

  1) There can be name conflicts, where two packages have functions with the same name. Using `::` ensures you're getting the function you want. 
  2) if you only want to use one function from a package, you can use `::` to skip the library call. As long as you've installed the package, you don't need to have loaded it to get the function. 
  3) For teaching purposes, it's nice to remind people where the function is coming from. 


{% highlight r %}
multiple_choice_responses <- readr::read_csv("multipleChoiceResponses.csv")
{% endhighlight %}



{% highlight text %}
## Warning in rbind(names(probs), probs_f): number of columns of result is not
## a multiple of vector length (arg 1)
{% endhighlight %}



{% highlight text %}
## Warning: 31 parsing failures.
## row # A tibble: 5 x 5 col     row col                   expected         actual file                 expected   <int> <chr>                 <chr>            <chr>  <chr>                actual 1  1344 LearningCategoryOnli… no trailing cha… .5     'multipleChoiceResp… file 2  2509 TimeModelBuilding     no trailing cha… .5     'multipleChoiceResp… row 3  2509 TimeVisualizing       no trailing cha… .5     'multipleChoiceResp… col 4  3236 LearningCategorySelf… no trailing cha… .5     'multipleChoiceResp… expected 5  3236 LearningCategoryOnli… no trailing cha… .5     'multipleChoiceResp…
## ... ................. ... .......................................................................... ........ .......................................................................... ...... .......................................................................... .... .......................................................................... ... .......................................................................... ... .......................................................................... ........ ..........................................................................
## See problems(...) for more details.
{% endhighlight %}


It's definitely faster, but it seems we have some errors. Let's inspect them. 


{% highlight r %}
problems(multiple_choice_responses)
{% endhighlight %}



{% highlight text %}
## # A tibble: 31 x 5
##      row col                  expected          actual file               
##    <int> <chr>                <chr>             <chr>  <chr>              
##  1  1344 LearningCategoryOnl… no trailing char… .5     'multipleChoiceRes…
##  2  2509 TimeModelBuilding    no trailing char… .5     'multipleChoiceRes…
##  3  2509 TimeVisualizing      no trailing char… .5     'multipleChoiceRes…
##  4  3236 LearningCategorySel… no trailing char… .5     'multipleChoiceRes…
##  5  3236 LearningCategoryOnl… no trailing char… .5     'multipleChoiceRes…
##  6  3579 TimeVisualizing      no trailing char… .5     'multipleChoiceRes…
##  7  3579 TimeFindingInsights  no trailing char… .5     'multipleChoiceRes…
##  8  4019 LearningCategorySel… no trailing char… .5     'multipleChoiceRes…
##  9  4019 LearningCategoryWork no trailing char… .5     'multipleChoiceRes…
## 10  4164 TimeProduction       no trailing char… .5     'multipleChoiceRes…
## # ... with 21 more rows
{% endhighlight %}

We see each row and column where a problem occurs. What's happening is that `read_csv` uses the first 1000 rows of a column to guess its type. But in some cases, it's guessing the column is an integer, because the first 1000 rows are whole numbers, when actually it should be double, as some entries have decimal points. We can fix this by changing the number of rows `read_csv` uses to guess the column type (with the `guess_max` argument) to the number of rows in the data set. 


{% highlight r %}
multiple_choice_responses <- readr::read_csv("multipleChoiceResponses.csv", 
                                             guess_max = nrow(multiple_choice_responses))
{% endhighlight %}

Error-free!

## Initial examination 

Let's see what we can glean from the column names themselves. I'll only look at the first 50 since there are so many. 


{% highlight r %}
colnames(multiple_choice_responses) %>%
  head(50)
{% endhighlight %}



{% highlight text %}
##  [1] "GenderSelect"                           
##  [2] "Country"                                
##  [3] "Age"                                    
##  [4] "EmploymentStatus"                       
##  [5] "StudentStatus"                          
##  [6] "LearningDataScience"                    
##  [7] "CodeWriter"                             
##  [8] "CareerSwitcher"                         
##  [9] "CurrentJobTitleSelect"                  
## [10] "TitleFit"                               
## [11] "CurrentEmployerType"                    
## [12] "MLToolNextYearSelect"                   
## [13] "MLMethodNextYearSelect"                 
## [14] "LanguageRecommendationSelect"           
## [15] "PublicDatasetsSelect"                   
## [16] "LearningPlatformSelect"                 
## [17] "LearningPlatformUsefulnessArxiv"        
## [18] "LearningPlatformUsefulnessBlogs"        
## [19] "LearningPlatformUsefulnessCollege"      
## [20] "LearningPlatformUsefulnessCompany"      
## [21] "LearningPlatformUsefulnessConferences"  
## [22] "LearningPlatformUsefulnessFriends"      
## [23] "LearningPlatformUsefulnessKaggle"       
## [24] "LearningPlatformUsefulnessNewsletters"  
## [25] "LearningPlatformUsefulnessCommunities"  
## [26] "LearningPlatformUsefulnessDocumentation"
## [27] "LearningPlatformUsefulnessCourses"      
## [28] "LearningPlatformUsefulnessProjects"     
## [29] "LearningPlatformUsefulnessPodcasts"     
## [30] "LearningPlatformUsefulnessSO"           
## [31] "LearningPlatformUsefulnessTextbook"     
## [32] "LearningPlatformUsefulnessTradeBook"    
## [33] "LearningPlatformUsefulnessTutoring"     
## [34] "LearningPlatformUsefulnessYouTube"      
## [35] "BlogsPodcastsNewslettersSelect"         
## [36] "LearningDataScienceTime"                
## [37] "JobSkillImportanceBigData"              
## [38] "JobSkillImportanceDegree"               
## [39] "JobSkillImportanceStats"                
## [40] "JobSkillImportanceEnterpriseTools"      
## [41] "JobSkillImportancePython"               
## [42] "JobSkillImportanceR"                    
## [43] "JobSkillImportanceSQL"                  
## [44] "JobSkillImportanceKaggleRanking"        
## [45] "JobSkillImportanceMOOC"                 
## [46] "JobSkillImportanceVisualizations"       
## [47] "JobSkillImportanceOtherSelect1"         
## [48] "JobSkillImportanceOtherSelect2"         
## [49] "JobSkillImportanceOtherSelect3"         
## [50] "CoursePlatformSelect"
{% endhighlight %}

We can see that there were categories of questions, like "Job Skill" and "Work Methods Frequency," with each method or skill having its own column.

Now let's take a look at our numeric columns with [skimr](https://github.com/ropensci/skimr). Skimr is a package from [rOpenSci](https://ropensci.org/) that allows you to quickly view summaries of your data. We  can use `select_if`  to select only columns where a certain condition, in this case whether it's a numeric column, is true.  


{% highlight r %}
multiple_choice_responses %>%
  select_if(is.numeric) %>%
  skimr::skim()
{% endhighlight %}



{% highlight text %}
## Skim summary statistics
##  n obs: 16716 
##  n variables: 13 
## 
## ── Variable type:integer ──────────────────────────────────────────────
##           variable missing complete     n  mean    sd p0 p25 p50 p75 p100
##                Age     331    16385 16716 32.37 10.47  0  25  30  37  100
##  TimeGatheringData    9186     7530 16716 36.14 21.65  0  20  35  50  100
##    TimeOtherSelect    9203     7513 16716  2.4  12.16  0   0   0   0  100
##      hist
##  ▁▅▇▃▁▁▁▁
##  ▅▅▅▇▂▂▁▁
##  ▇▁▁▁▁▁▁▁
## 
## ── Variable type:numeric ──────────────────────────────────────────────
##                       variable missing complete     n  mean    sd p0 p25
##         LearningCategoryKaggle    3590    13126 16716  5.53 11.07  0   0
##  LearningCategoryOnlineCourses    3590    13126 16716 27.38 26.86  0   5
##          LearningCategoryOther    3622    13094 16716  1.8   9.36  0   0
##    LearningCategorySelftTaught    3607    13109 16716 33.37 25.79  0  15
##     LearningCategoryUniversity    3594    13122 16716 16.99 23.68  0   0
##           LearningCategoryWork    3605    13111 16716 15.22 19     0   0
##            TimeFindingInsights    9193     7523 16716 13.09 12.97  0   5
##              TimeModelBuilding    9188     7528 16716 21.27 16.17  0  10
##                 TimeProduction    9199     7517 16716 10.81 12.26  0   0
##                TimeVisualizing    9187     7529 16716 13.87 11.72  0   5
##  p50 p75 p100     hist
##    0  10  100 ▇▁▁▁▁▁▁▁
##   20  40  100 ▇▃▂▃▁▁▁▁
##    0   0  100 ▇▁▁▁▁▁▁▁
##   30  50  100 ▇▇▅▇▂▁▁▂
##    5  30  100 ▇▂▁▁▁▁▁▁
##   10  25  100 ▇▂▁▁▁▁▁▁
##   10  20  303 ▇▁▁▁▁▁▁▁
##   20  30  100 ▇▇▃▂▁▁▁▁
##   10  15  100 ▇▂▁▁▁▁▁▁
##   10  20  100 ▇▅▁▁▁▁▁▁
{% endhighlight %}

I love the histograms. We can quickly see from the histograms that people self teach a lot and spend a good amount of time building models and gathering data, compared to visualizing data or working in production.   

Let's see how many distinct answers we have for each question. We can use `n_distinct()`, a shorter and faster version of `length(unique())`, and `map_df` once again to apply the function to every column. 


{% highlight r %}
multiple_choice_responses %>%
  summarise_all(n_distinct) %>%
  select(1:10)
{% endhighlight %}



{% highlight text %}
## # A tibble: 1 x 10
##   GenderSelect Country   Age EmploymentStatus StudentStatus
##          <int>   <int> <int>            <int>         <int>
## 1            5      53    85                7             3
## # ... with 5 more variables: LearningDataScience <int>, CodeWriter <int>,
## #   CareerSwitcher <int>, CurrentJobTitleSelect <int>, TitleFit <int>
{% endhighlight %}

This data would be more helpful if it was tidy and had two columns, `question` and `num_distinct_answers`. We can use `tidyr::gather` to change our data from "wide" to "long" format and then `arrange` it so we can see the columns with the most distinct answers first. If you've used (or are still using!) reshape2, check out tidyr; reshape2 is retired and is only updated with changes necessary for it to remain on CRAN. While not exactly equivalent, `tidyr::spread` replaces `reshape2::dcast`, `tidyr::separate` `reshape2::colsplit`, and `tidyr::gather` `reshape2::melt`. 


{% highlight r %}
multiple_choice_responses %>%
  summarise_all(n_distinct) %>%
  tidyr::gather(question, num_distinct_answers) %>%
  arrange(desc(num_distinct_answers))
{% endhighlight %}



{% highlight text %}
## # A tibble: 228 x 2
##    question               num_distinct_answers
##    <chr>                                 <int>
##  1 WorkMethodsSelect                      6191
##  2 LearningPlatformSelect                 5363
##  3 WorkToolsSelect                        5249
##  4 WorkChallengesSelect                   4288
##  5 WorkDatasetsChallenge                  2221
##  6 PastJobTitlesSelect                    1856
##  7 MLTechniquesSelect                     1802
##  8 WorkDatasets                           1724
##  9 WorkAlgorithmsSelect                   1421
## 10 MLSkillsSelect                         1038
## # ... with 218 more rows
{% endhighlight %}

Let's take a look at the question with the most distinct answers, WorkMethodsSelect. 


{% highlight r %}
multiple_choice_responses %>%
  count(WorkMethodsSelect, sort = TRUE)
{% endhighlight %}



{% highlight text %}
## # A tibble: 6,191 x 2
##    WorkMethodsSelect                           n
##    <chr>                                   <int>
##  1 <NA>                                     8943
##  2 Data Visualization                        144
##  3 Other                                     144
##  4 Logistic Regression                        66
##  5 Time Series Analysis                       49
##  6 Neural Networks                            45
##  7 A/B Testing                                42
##  8 Data Visualization,Time Series Analysis    37
##  9 Text Analytics                             36
## 10 Decision Trees                             29
## # ... with 6,181 more rows
{% endhighlight %}

We can see this is a multiple select question, where if a person selected multiple answers they're listed as one entry, separated by commas. Let's tidy it up. 

First, let's get rid of the NAs. We can use `!is.na(WorkMethodsSelect)`, short for `is.na(WorkMethodsSelect) == FALSE`, to filter out NAs. We then use `str_split`, from stringr, to divide the entries up. `str_split(WorkMethodsSelect, ",")`  says "Take this string and split it into a list by dividing it where there are `,`s." 


{% highlight r %}
nested_workmethods <- multiple_choice_responses %>%
  select(WorkMethodsSelect) %>%
  filter(!is.na(WorkMethodsSelect)) %>%
  mutate(work_method = str_split(WorkMethodsSelect, ",")) 

nested_workmethods %>%
  select(work_method)
{% endhighlight %}



{% highlight text %}
## # A tibble: 7,773 x 1
##    work_method
##    <list>     
##  1 <chr [5]>  
##  2 <chr [12]> 
##  3 <chr [17]> 
##  4 <chr [14]> 
##  5 <chr [12]> 
##  6 <chr [1]>  
##  7 <chr [14]> 
##  8 <chr [12]> 
##  9 <chr [7]>  
## 10 <chr [5]>  
## # ... with 7,763 more rows
{% endhighlight %}

Now we have a list column, with each entry in the list being one work method. We can `unnest` this so we can get back a tidy dataframe. 


{% highlight r %}
unnested_workmethods <- nested_workmethods %>%
  tidyr::unnest(work_method) %>%
  select(work_method)

unnested_workmethods
{% endhighlight %}



{% highlight text %}
## # A tibble: 59,497 x 1
##    work_method                     
##    <chr>                           
##  1 Association Rules               
##  2 Collaborative Filtering         
##  3 Neural Networks                 
##  4 PCA and Dimensionality Reduction
##  5 Random Forests                  
##  6 A/B Testing                     
##  7 Bayesian Techniques             
##  8 Data Visualization              
##  9 Decision Trees                  
## 10 Ensemble Methods                
## # ... with 59,487 more rows
{% endhighlight %}

Great! As a last step, let's `count` this data so we can find which are the most common work methods people use. 


{% highlight r %}
unnested_workmethods %>%
  count(work_method, sort = TRUE)
{% endhighlight %}



{% highlight text %}
## # A tibble: 31 x 2
##    work_method                          n
##    <chr>                            <int>
##  1 Data Visualization                5022
##  2 Logistic Regression               4291
##  3 Cross-Validation                  3868
##  4 Decision Trees                    3695
##  5 Random Forests                    3454
##  6 Time Series Analysis              3153
##  7 Neural Networks                   2811
##  8 PCA and Dimensionality Reduction  2789
##  9 kNN and Other Clustering          2624
## 10 Text Analytics                    2405
## # ... with 21 more rows
{% endhighlight %}

We see the classic methods of data visualization, logistic regression, and cross-validation lead the pack. 

### Graphing Frequency of Different Work Challenges 

Now let's move on to understanding what challenges people face at work. This was one of those categories where there were multiple questions asked, all having names starting with `WorkChallengeFrequency` and ending with the challenge (e.g "DirtyData"). 

We can find the relevant columns by using the dplyr `select` helper `contains`. We then use `gather` to tidy the data for analysis, filter for only the non-NAs, and remove the `WorkChallengeFrequency` from each question using `stringr::str_remove`. 


{% highlight r %}
WorkChallenges <- multiple_choice_responses %>%
  select(contains("WorkChallengeFrequency")) %>%
  gather(question, response) %>%
  filter(!is.na(response)) %>%
  mutate(question = stringr::str_remove(question, "WorkChallengeFrequency")) 

WorkChallenges
{% endhighlight %}



{% highlight text %}
## # A tibble: 31,486 x 2
##    question response        
##    <chr>    <chr>           
##  1 Politics Rarely          
##  2 Politics Often           
##  3 Politics Often           
##  4 Politics Often           
##  5 Politics Rarely          
##  6 Politics Most of the time
##  7 Politics Often           
##  8 Politics Often           
##  9 Politics Rarely          
## 10 Politics Most of the time
## # ... with 31,476 more rows
{% endhighlight %}

Let's make a facet bar plot, one for each question with the frequency of responses.To make the x-axis tick labels readable, we'll change them to be vertical instead of horizontal. 


{% highlight r %}
ggplot(WorkChallenges, aes(x = response)) + 
  geom_bar() + 
  facet_wrap(~question) + 
  theme(axis.text.x = element_text(angle = 90, hjust = 1))
{% endhighlight %}

![center](/figs/2018-09-26-Lesser-Known-Stars-of-the-Tidyverse/WorkChallenges_graph1-1.png)

This graph has two main problems. First, there are too many histograms for it to be really useful. But second, the order of the x-axis is wrong. We want it to go from least often to most, but instead `rarely` is in the middle. We can manually reorder the level of this variable using `forcats::fct_relevel`. 


{% highlight r %}
WorkChallenges %>%
  mutate(response = fct_relevel(response, "Rarely", "Sometimes", 
                                "Often", "Most of the time")) %>%
  ggplot(aes(x = response)) + 
  geom_bar() + 
  facet_wrap(~question) + 
  theme(axis.text.x = element_text(angle = 90, hjust = 1))
{% endhighlight %}

![center](/figs/2018-09-26-Lesser-Known-Stars-of-the-Tidyverse/WorkChallenges_graph2-1.png)

Now we've got the x-axis in the order we want it. Let's try dichotomizing the variable by grouping "most of the time" and "often" together as the person considering something a challenge. We can use `if_else` and `%in%`. `%in%` is equivalent to `response == "Most of the time" | response == "Often"` and can save you a lot of typing if you have a bunch of variables to match. 

Grouping by the question, we can use `summarise` to reduce the dataset to one row per question, adding the variable `perc_problem` for the percentage of responses that thought something was a challenge often or most of the time. This way, we can make one graph with data for all the questions and easily compare them. 


{% highlight r %}
perc_problem_work_challenge <- WorkChallenges %>%
  mutate(response = if_else(response %in% c("Most of the time", "Often"), 1, 0)) %>%
  group_by(question) %>%
  summarise(perc_problem = mean(response)) 
{% endhighlight %}


{% highlight r %}
ggplot(perc_problem_work_challenge, aes(x = question, y = perc_problem)) + 
  geom_point() +
  coord_flip()
{% endhighlight %}

![center](/figs/2018-09-26-Lesser-Known-Stars-of-the-Tidyverse/perc_problem_work_challenge_graph-1.png)

This is better, but it's hard to read because the points are scattered all over the place. Although you can spot the highest one, you then have to track it back to the correct variable. And it's also hard to tell the order of the ones in the middle. 

We can use `forcats:fct_reorder` to change the x-axis to be ordered by another variable, in this case the y-axis. While we're at it, we can use `scale_y_continuous` and`scales::percent` to update our axis to display in percent and `labs` to change our axis labels. 


{% highlight r %}
ggplot(perc_problem_work_challenge, 
       aes(x = perc_problem, 
           y = fct_reorder(question, perc_problem))) + 
  geom_point() +
  scale_x_continuous(labels = scales::percent) + 
  labs(y = "Work Challenge", x = "Percentage of people encountering challenge frequently")
{% endhighlight %}

![center](/figs/2018-09-26-Lesser-Known-Stars-of-the-Tidyverse/perc_problem_work_challenge_graph2-1.png)

Much better! You can now easily tell which work challenges are encountered most frequently. 

### Conclusion

I'm a big advocate of using and teaching the tidyverse for data analysis and visualization in R (it runs [in the family](http://varianceexplained.org/r/teach-tidyverse/)). In addition to doing these talks, I've just released a DataCamp course on [Categorical Data in the Tidyverse](https://www.datacamp.com/courses/categorical-data-in-the-tidyverse). I walk through some of the functions in this course and more from forcats. It's part of the new Tidyverse Fundamentals skill track, which is suitable for people are new to R or those looking to switch to the tidyverse. Check it out and let us know what you think. 

Some other good resources of learning the tidyverse are Hadley Wickham and Garrett Grolemund's free [R for Data Science book](http://r4ds.had.co.nz/) and [RStudio's cheat sheets](https://www.rstudio.com/resources/cheatsheets/). If you have questions, I recommend using the [tidyverse section of RStudio community](https://community.rstudio.com/c/tidyverse) and/or the #rstats hashtag on Twitter. If you do, make sure you include a reproducible example (see best practices [here](https://reprex.tidyverse.org/articles/reprex-dos-and-donts.html)) with the [reprex package](https://reprex.tidyverse.org/articles/articles/magic-reprex.html)!

