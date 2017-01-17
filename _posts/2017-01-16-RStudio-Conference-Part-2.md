This is the second part of my posts on the rstudio::conf. If you're interested in more general thoughts on the conference and some personal notes, check out the first post(LINK). This post is to gather, as succintly and organized as possible, the practical and technical things I learned at the conference that will hopefully also be useful to others.  

## Some Packages, Tools, and Functions I Learned
* **Assertr's verify function**: The verify function is meant for use in a data analysis pipeline. It raises an error if the logical within the function is false and just returns the data if True. This is a great way to add some assumption checks in your data pipelines. Here's an example:

```
mtcars %>% 
  verify(nrow(.) == 32) %>%
  filter(cyl == 6)
```
 
* This simply returns the data frame of all cars with 6 cylinders, as expected, because `mtcars` does indeed have 32 rows. If, however, we had put the wrong number of rows in (e.g. `verify(nrow(.) == 24)`, we would have gotten no data, with this error instead: `Error in verify(., nrow(.) == 23) : verification failed! (1 failure)`. 

* **Profvis**: This is a tool to visualize how long each part of your code is taking to run. This is a great way to figure out how to speed up your code, as often your intuition of what is taking the most time does not match reality. To use it, all you have to do is wrap your code in the profvis function, like so: `profvis({ my_function })`. A new pane will then pop up in RStudio that shows how long each line takes to run and even what functions each calls. Learn more on the RStudio [Profvis page](https://rstudio.github.io/profvis/). 

(screenshot here)

* **Abbreviating arguments**: You can abbreviate arguments within functions. So instead of writing `mean(c(10, 5, NA), na.rm = TRUE`), you can simply right `mean(c(10, 5, NA), n = T)`. 

* **Listviewer**: One of the hardest parts about working with nested lists is trying to figure out what the heck is in them. The `jsonedit` function from listviewer allows you to see the layers of a list and even search through them. Here's what it looks like when I run it with got_chars, a list from Jenny Bryan's great [repurrrsive package](https://github.com/jennybc/repurrrsive):

(Screenshot here) 

## Keyboard Shortcuts

* Hit tab after you start typing to get all functions that start with those letters. Cmd/Ctrl + up arrow instead gives you the commands you've typed
* Alt + shift + k for all keyboard shortcuts

## Hadley's General Tips and Tricks

* Reading rcode broadly is useful, as it can help expand your R vocabulary. 
* Haldey sets these options in R, to avoid the pitfalls of R's "helpful" partial name matching

```
options(warnPartialMatchArgs = TRUE, warnPartialMatchDollar = TRUE, warnPartialMatchAttr = TRUE)
```

* Don't proactively worry about performance of your code, but whether it's clear. Don't try to read your code and think whether it will be fast or slow. Your intuition is terrible  just run it! You can also use `profvis` to help. 
* It's very easy to make code faster by making it incorrect. One of the reasons to write tests!
* Restart R a few times a day and never restore or save your .RData. This will help the reproducibility of your code. 
* Don't use comments to see what/how your code is doing, use it to describe why. Otherwise, you have to remember to change comments when you change your code. You really don't want to end up with your code doing one thing and your comment saying you're doing something else.
* You can be too verbose in your code because don't have enough r vocabulary. For example: 
    - if `(x == TRUE)` is the same as `if(x)`
    - `y == "a" | y == "b" | y == "c"` is the same as `y %in% c("a", "b", “c”)`
