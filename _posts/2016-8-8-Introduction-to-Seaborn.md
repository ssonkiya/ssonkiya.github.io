### The Bright Blue Horror

Coming into Metis, I knew one of the hardest parts would be switching from R to Python. Beyond simply having much more experience in R, I had come to rely on Hadley Wickham's great set of R packages for data science. One of these is ggplot2, a data visualizaiton package. While there is a version of [ggplot2 for python](http://ggplot.yhathq.com), I decided to learn the main plotting system in Python, matplotlib. Then I actually created and saw my first matplotlib:

<p align="center">
  <img src="https://github.com/robinsones/robinsones.github.io/blob/draft-post-3/images/blog_post_ugly_plot.png" alt="Sublime's custom image"/>
</p>

I promptly sent my brother the following texts: 

<p align="center">
  <img src="https://github.com/robinsones/robinsones.github.io/blob/draft-post-3/images/Dave-test.png" alt="Sublime's custom image"/>
</p>

Fortunately, he wrote back quickly suggesting I try Seaborn, and my bootcamp experience was saved. Six weeks later, I've become known in Metis as a seaborn evangelicalist. Not a plot on presentation days goes by without me marking down if it uses base matplotlib aesthetics. I then follow up with those people, asking I ask them why they don't use seaborn, usually followed by, “Is it to make me sad?"

### The Great ggplot2 Versus Base R Wars

<p align="center">
  <img src="https://github.com/robinsones/robinsones.github.io/blob/draft-post-3/images/joker_pic.png" alt="Sublime's custom image"/>
</p>

Back in February, Jeff Leek, an Associate Professor of Biostatistics and Oncology at John Hopkins and co-director of the popular John Hopkins specialization in data science on cousera, made a blog post entitled ["Why I don't use ggplot2"](http://simplystatistics.org/2016/02/11/why-i-dont-use-ggplot2/). The [post wasn't even out two days before by my brother, David Robinson, one of the people Jeff Leek had called out for giving him grief about not using ggplot2, followed up with a post of his own on [why he uses ggplot2]((http://varianceexplained.org/r/why-I-use-ggplot2/). Another data scientist, Nathan Yau, joined in a month later with an [extensive post](https://flowingdata.com/2016/03/22/comparing-ggplot2-and-r-base-graphics/) comparing the two for a variety of plots. Ben Casselman of FiveThirtyEight joined the fray in a ["tweetstorm"](https://twitter.com/bencasselman/status/712405057388601344). It settled down after that, but still simmers under the surface, bubbling up at the Joint Statistical Meetings this month.

I bring this up not only to illustrate that strong opinions about plotting seems to run in the family, but also to set up a contrast to seaborn versus matplotlib. While Base R graphics and ggplot2 require completely different syntax, seaborn is *based on* matplotlib, and so starting to use seaborn is as easy as importing it. 

### Introduction to Seaborn

[Seaborn](https://stanford.edu/~mwaskom/software/seaborn/) is a data visualization library in Python based on matplotlib. The seaborn website has some great documentation, including a [tutorial](https://stanford.edu/~mwaskom/software/seaborn/tutorial.html). And like the rest of your programming questions, anything you can't find on this website can generally be found on the Stack Overflow page that is your first result when you google the question. 










