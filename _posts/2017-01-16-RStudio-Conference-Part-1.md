Last week I spent four amazing days in Orlando at the first ever [rstudio::conf](https://www.rstudio.com/conference/). I learned a ton, met some really cool people and connected with those I hadn't seen in a while, and came out feeling ready to take on the world. I learned so much in fact that I've divided this post into two parts. This first one shares my personal experience and some more general learnings, while the second one has quick, bullet-pointed lists on writing functions, some packages, tools, and functions I learned, and general tips and tricks from Hadley (include link here). I also learned a lot about writing packages on the second day of Hadley's Master R Developer workshop, which will be included in my next post on writing my first package.

You can find most of the slides from the conference [here](https://github.com/kbroman/RStudioConf2017Slides), organized by Karl Broman (who was greatly missed). RStudio will also be posting video from most (all?) of the talks, which I'll add here when they're available. 

## Diversity and Inclusion in R
First, I want to thank RStudio for providing the diversity scholarship that enabled me to attend the conference at all. When I applied for the scholarship, I was just finishing up [Metis](http://www.thisismetis.com/data-science-bootcamps) and about to start my job search. Without a company to pay for my ticket and not knowing when I would start earning a salary again, I could not have covered the $1000+ for the conference ticket and travel. 

The scholarship covered not only the cost of the conference, the Harry Potter World social event, and a small amount of travel money, but also, to my surprise, a ticket for the training days. The training days cost $1,300, so even though I'm now at Etsy, which has some conference funding available, I do not think there's any other way I would have been able to attend this. 

The availability of rstudio::conf diversity scholarships for women, LGBT+, and underrepresented minorities reflects the broader inclusion efforts I've seen the R community. We have the newly-rebranded [R Forwards taskforce](http://forwards.github.io/), which changed their name from the R Foundation Task Force on Women so that they could "consider the needs of other marginalised groups as part of our mission." There is also the [R-Ladies organization](https://rladies.org/), which has meet-up groups all around the world. [UseR!](http://www.user2017.brussels/), one of the main R conferences, is providing childcare options this year. Overall, members of the R community seem aware of diversity issues and supportive of efforts to make the community more inclusive. That being said, there's always more work to be done, as emphasized in this [report](https://forwards.github.io/blog/2017/01/13/mapping-users/) on the demographics of the useR! attendees. I hope to do some of my part for these efforts by giving at least one R tutorial for the RLadies New York group this spring. 

I met many kick-ass R Ladies at the conference, including Jenny Bryan, Charlotte Wickham, Hilary Parker, and these awesome graduate students from Vanderbuilt: 

![center](https://github.com/robinsones/robinsones.github.io/blob/rstudioconf-draft-post/images/Harry_Potter_Shirts.png)

One person I was especially excited to meet was Julia Silge, who works with [my brother](varianceexplained.org) at StackOverflow. I'd been following her on Twitter and reading her blog for a while, but we had never met in person. In fact, this conference was only the second time she met Dave in person, even though he hired her for StackOverflow and developed the tidy text package together! 

![center](https://github.com/robinsones/robinsones.github.io/blob/master/images/Julia_tweet.png)

Julia only started using R about a year and a half ago, but she already has a great and active blog, a popular package, and a new career as a Data Scientist. While her strong previous scientific training certainly helped her, she's a great example of how you can transition into data science later in your career without necessarily having a statistics or computer science degree.

## Training Days

There were three training day workshops available: Master R Developer with Hadley Wickahm, Intermediate Shiny with Joe Cheng, and Master the Tidyverse with Garrett Grolemund. The Tidyverse workshop was more basic, and while I definitely want to learn more Shiny, I decided to take the Master R Developer workshop. I've been meaning to learn more about making packages, the focus of the second day, and I always enjoy getting the chance to learn from Hadley again: 

![center](https://github.com/robinsones/robinsones.github.io/blob/master/images/Hadley_learning.png)
The first day had some material I'd heard already in meetups Hadley has done, but it's always helpful to get a refresher. Other material, such as those on Object-Oriented Programming and S3 classes, were new. The second day on packages was entirely new to me and a great way to help overcome inertia and finally start writing packages. 

There were about 70 people in the Master R Developer workshop. Hadley ran a pretty interactive workshop, with lots of exercises to work on yourself or with your neighbor. This was a great way to try out what you'd just learn, figure out what you didn't understand, and get help from the couple of TAs if needed. 

