Last week I spent four amazing days in Orlando at the first ever [rstudio::conf](https://www.rstudio.com/conference/). I learned a ton, met some really cool people, connected with those I hadn't seen in a while, and came out feeling ready to take on the world. I've divided this post into two parts. This first one shares my personal experience and some more general learnings, while the second one has quick, bullet-pointed lists on writing functions, some packages, tools, and functions I learned, and general tips and tricks from Hadley (include link here). I also learned a lot about writing packages on the second day of Hadley's Master R Developer workshop, which will be included in my next post on writing my first package.

You can find most of the slides from the conference [here](https://github.com/kbroman/RStudioConf2017Slides), organized by Karl Broman (who was greatly missed). RStudio will also be posting videos of the talks, which I'll add here when they're available. 

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

There were three training day workshops available: Master R Developer with Hadley Wickham, Intermediate Shiny with Joe Cheng, and Master the Tidyverse with Garrett Grolemund. The Tidyverse workshop was more basic, and while I definitely want to learn more Shiny, I decided to take the Master R Developer workshop. I've been meaning to learn more about making packages, the focus of the second day, and I always enjoy getting the chance to learn from Hadley again: 

![center](https://github.com/robinsones/robinsones.github.io/blob/master/images/Hadley_learning.png)

The first day had some material I'd heard already in meetups Hadley has done, but it's always helpful to get a refresher. Other material, such as those on Object-Oriented Programming and S3 classes, were new. The second day on packages was entirely new to me and a great way to help overcome inertia and finally start writing packages. 

There were about 70 people in the Master R Developer workshop. Hadley ran a pretty interactive workshop, with lots of exercises to work on yourself or with your neighbor. This was a great way to try out what you'd just learn, figure out what you didn't understand, and get help from the couple of TAs if needed. 

A good amount of the information he covered is available in two of his free online books, [Advanced R](http://adv-r.had.co.nz/) and [R Packages](http://r-pkgs.had.co.nz/). But I personally prefer learning programming from a lecture and interactive exercises to reading a book, and going to the training days also forced me to work on the material, rather than putting it off for some day. Moreover, going in person allows you to ask questions (and learn from others' questions). Hadley also made some off-the-cuff remarks that, because of his long experience with R, were very helpful. Finally, the workshop was just plain enjoyable, and a great way to spend two days. 

## Main Conference 

Outside of the two keynotes and closing panel, the conference had two parallel tracks of talks. This meant I couldn’t get to them all, although I plan to watch a few I missed once the videos are uploaded. It was a great, diverse line-up of speakers. Some of my favorites was Hilary’s talk on [Opinionated Analysis Development](http://www.slideshare.net/hilaryparker/opinionated-analysis-development), Bob Rudis’ on [Writing Readable Code with Pipes](https://github.com/hrbrmstr/rstudioconf2017#readme), Jenny Bryan’s on [List-Columns](https://speakerdeck.com/jennybc/putting-square-pegs-in-round-holes-using-list-cols-in-your-dataframe), and Julia Silge’s on [Tex Mining, the Tidy Way](https://speakerdeck.com/juliasilge/text-mining-the-tidy-way), and Jonathan McPherson on R Notebook Workflow. If you’re very interested, I would suggest watching the videos rather than just reading the slides in order to get the full impact; each talk is only about 20 minutes. I obviously picked up a lot of technical tips and tricks from the content of the talks (see my other post for these), but I could always have watched these later online. What I couldn’t have gotten is a full immersion into the R community. 

To me, getting to meet and talk with new people and non-New York friends was one of the best parts of the conference. I think one story nicely illustrates some of the spontaneous connections that can be formed at these conferences. At lunch one day I met James, a Columbia professor, and we realized we were on the same flight back to New York. He asked how I was getting to the airport, and I told him I was sharing a cab with my brother, whose work was paying for the ride, and asked if he wanted to join us. 

After a quick introduction to my brother, who was running around talking to other people, we all met up again Saturday night to share the cab. He and Dave quickly hit it off, with Dave noticing he seems to know a lot about one of his packages, gganimate. About halfway through the ride, James said “Yeah, a couple months ago I used it to [animate NBA plays”](https://twitter.com/revodavid/status/771747696617160704). At which point, David goes “You’re James Curley, aren’t you?” It turns out Dave had been following James on twitter for a while and meant to find him at the conference. They spent the next few hours as we waited for our delayed flight in animated discussion while I enjoyed playing Super Mario Run. 

## Harry Potter World

I’d be remiss if I didn’t mention the amazing night at Harry Potter world, where we had all of Hogsmeades to ourselves, as RStudio had rented out the park for the evening. Hillary Parker took some [beautiful pictures](https://photos.google.com/share/AF1QipOe6Ypp_WLkOcBJQzxXhOY2RNelv8w57eR285pZuDvBxedg1liRCvaijNJsqgLeWw?key=WFNTVUNyRnlsOTlaVkpqS1pibFFhUE82MlVBTnVB) on her DSLR that captured the mood: 

![center](https://github.com/robinsones/robinsones.github.io/blob/rstudioconf-draft-post/images/HP_image1.JPG)

It was a bit bizarre to go on a roller coaster that had more staff members than riders, but I definitely didn’t miss the lines. I was even picked to get a free wand from Ollivander's, which you can use to interact with the shop windows. But again, whether exploring Hogwarts, enjoying a butter beer, or grabbing the front row of a ride, the experience was made all the richer for the people I got to enjoy it with: 

![center](https://github.com/robinsones/robinsones.github.io/blob/rstudioconf-draft-post/images/Group_HP_pic.JPG)


## Wrap-Up

For those considering going to rstudio::conf next year, I hope this gave you a better idea of what the experience is like. I want to emphasize again how friendly everyone at this conference was, including prominent members of the community. Don’t be shy about introducing yourself after talks or at the lunch table. 

In the final panel of Hadley Wickham, J.J. Allaire, and Joe Cheng, one of them said: “the strength of R is in the human factors. It's possible for those who don’t have a conventional software background to be successful with R.” The R language creators, RStudio, and the many open-source package developers deserve much of the credit for this, but I think the final piece of really making R accessible is the rest of great and friendly community. Whether it’s StackOverflow power-users who enable you to often find the answer to your question in your a google result, or the active bloggers who step you through their code and analysis, or twitter users who share the newest packages and their opinions about doing analysis, there's always people ready to help.  
