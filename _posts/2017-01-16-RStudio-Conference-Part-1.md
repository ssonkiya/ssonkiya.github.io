Last week I spent four amazing days in Orlando at the first ever [rstudio::conf](https://www.rstudio.com/conference/). In this post, I share some of what I learned from the conference along with my personal experience. Since it's a long post,  I’ve divided it up into sections to make it easier to navigate if you’re just interested in part of the post:

* Some Notes on Diversity 
* Helpful Links Related to the Conference
* Training Days
* Functional Programming
* Writing Packages
* Hadley's Tips and Tricks
* Some Packages, Tools, and Functions I learned 
* Tips and Tricks from the Tutorials 
* Keyboard Shortcuts 
* Personal Notes
* In the Future

## Some Notes on Diversity
First, I want to thank RStudio for providing the diversity scholarship that enabled me to attend the conference at all. When I applied for the scholarship, I was just finishing up [Metis](http://www.thisismetis.com/data-science-bootcamps) and about to start my job search. Without a company to pay for my ticket and not knowing when I would start earning a salary again, I could not have covered the $1000+ for the conference ticket and travel. 

The scholarship covered not only the cost of the conference, the Harry Potter World social event, and a small amount of travel money, but also, to my surprise, a ticket for the training days. The training days cost $1,300, so even though I'm now at Etsy, which has some conference funding available, I do not think there's any other way I would have been able to attend this. 

The availability of diversity scholarships for women, LGBT+, and underrepresented minorities reflects the broader inclusion efforts I've seen the R community. There is the newly-rebranded R Forwards taskforce, which changed from the R Foundation Task Force on Women so that they could "consider the needs of other marginalised groups as part of our mission." There is the [R-Ladies organization](https://rladies.org/), which has meet-up groups all around the world. UseR!, one of the main R conferences, is providing childcare options this year, and in general, members of the R community seem aware of diversity issues and supportive of efforts to make the community more inclusive. That being said, there's always more work to be done, as emphasize in this [report](https://forwards.github.io/blog/2017/01/13/mapping-users/) on the demographics of the useR! attendees. 

I loved meeting up with awesome RLadies at conference, including Jenny Bryan, Charlotte Wickham, Hilary Parker, and these cool graduate students from Vanderbuilt: 

(link to tweet with t-shirts)

One person I was especially excited to meet was Julia Silge, who works with my brother at StackOverflow. I'd been following her on Twitter for a while but we'd never met in person. In fact, this conference was only the second time she met Dave in person, even though he hired her for StackOverflow and developed the tidy text package together! She's very inspirational to me: she only started using R about a year and a half ago, and she already has a great and active blog, a popular package, and a new career as a Data Scientist. While her strong previous scientific training certainly helped her, she's a great example of how you don't necessariliy need a statistics or computer science degree to become a data scientist. 

(link to Julia's tweet)

## Helpful and Interesting Links 
* [Slides](https://github.com/kbroman/RStudioConf2017Slides) from the conference, organized by Karl Broman (who was greatly missed) 
* Stephen Turner's [recap](http://www.gettinggeneticsdone.com/2017/01/rstudio-conference-2017-recap.html)
* Sharon Machlis's [tips and takeaways](http://www.computerworld.com/article/3157004/data-analytics/best-tips-and-takeaways-from-rstudio-conference.html)
* Simon Jackson's [takeaways](https://drsimonj.svbtle.com/opinions-and-challenges-at-rstudio-conf) from the conference, organized around the opinions and challenges about data science processes in the R community. This organizational schema was inspired by Hilary Parker's great talk on [Opinionated Analysis Development](http://www.slideshare.net/hilaryparker/opinionated-analysis-development))
* RStudio will also be posting video from most (all?) of the talks, which I'll add here when they're available

## Training Days

There were three training day workshops available: Master R Developer with Hadley Wickahm, Intermediate Shiny with Joe Cheng, and Master the Tidyverse with Garrett Grolemund. The Tidyverse workshop was more basic, and while I definitely want to learn more Shiny, I decided to take the Master R Developer workshop. I've been meaning to learn more about making packages, the focus of the second day, and I always enjoy getting the chance to learn from Hadley again: 

(link to tweet about learning from Hadley)

The first day had some material I'd heard already in meetups Hadley has done, but it's always helpful to get a refresher. Other material, such as those on Object-Oriented Programming and S3 classes, were new. The second day on packages was entirely new to me and a great way to help overcome inertia and finally start writing packages. 

There were about 70 people in the Master R Developer workshop. Hadley ran a pretty interactive workshop, with lots of exercises to work on yourself or with your neighbor. This was a great way to try out what you'd just learn, figure out what you didn't understand, and get help from the couple of TAs if needed. 

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

* Don't proactively worry about performance of your code, but whether it's clear. 
* Restart R a few times a day and never restore or save your .RData. This will help the reproducibility of your code. 
* Don't use comments to see what/how your code is doing, use it to describe why. Otherwise, you have to remember to change comments when you change your code. You really don't want to end up with your code doing one thing and your comment saying you're doing something else.
* Don't try to read your code and think whether it will be fast or slow. Your intuition is terrible  just run it! You can also use `profvis` to help. 
* It's very easy to make code faster by making it incorrect. One of the reasons to write tests!
* You can be too verbose in your code because don't have enough r vocabulary. For example: 
    - if `(x == TRUE)` is the same as `if(x)`
    - `y == "a" | y == "b" | y == "c"` is the same as `y %in% c("a", "b", “c”)`



