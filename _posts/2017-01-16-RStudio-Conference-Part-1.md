Last week I spent four amazing days in Orlando at the first ever [rstudio::conf](https://www.rstudio.com/conference/). I learned a ton, met some really cool people and connected with those I hadn't seen in a while, and came out feeling ready to take on the world. I learned so much in fact that I've divided this post into two parts. This first one shares my personal experience and some more general learnings, while the second one has quick, bullet-pointed lists on writing functions, some packages, tools, and functions I learned, and general tips and tricks from Hadley. (include link here). My next post will be on writing my first R package and will include a lot of the tips I learned from Hadley in the second day of the Master R Developer workshop. 

## Diversity and Inclusion in R
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

