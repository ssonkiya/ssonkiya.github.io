---
title: When Small Businesses Make it Big
permalink: Small-Businesses-Make-it-Big
---

For my unsupervised NLP project, I decided to take a look at small business and entrepreneurship articles to see if I could identify trends and changes in them over time.

With a relatively easy to use API, and access to a multitude of articles across decades, New York Times was my newpaper of choice for the project.

I filtered for articles that matched on keywords of "Entrepreneurship", "Small Business", and "Venture Capitalism" since I they yielded the most results and seemed to encapsulate small businesss.


Unfortunately, and also, kind of surprisingly, it was incredibly difficult to get a large number of articles. NYT doesn't publish many articles with those tags, and in total I parsed about 8,000 articles from 1970 to current. Of all of these, there were articles that, though might have been for entrepreneurs, were briefings and overviews about what was going on in the world. Since global news was definitely not what I was looking for, and was going to dirty my topic modelling, I threw them out. This finally lead me to work with about 5,500 articles. 


This is what the article distribution by year looked like:

![too big](/images/NLP/article_dist.png)


I felt this unequal distribution plagued me throughout the project. I had many more articles in the recent years, than from back then.

I cleaned by articles with tokenization, and the general preprocessing. I used TFIDF with a max_df of 0.4. This is just the hyperparameter that seemed to best parse out different topics for me. 
Running an NMF yielded me these topics:

![too big](/images/NLP/topic_modelling.png)


The topic delination seemed to be pretty nice, disregarding what I called the general_business topic. This seems to be the catch all topic, since the words definitely didn't describe a topic as strongly as the other groups.


I didn't want to classify each document to a topic by using something like max topic weight. Instead I looked at the mean percent of topic each article talked about, grouped by year.


![too big](/images/NLP/tableau_topic_percent.png)


What this means, is that in 2015-2018, on average ~30% of articles were about gen_business, and 16% of the articles were related to the silicon_valley topic etc. So you can see an overview of how topics changed over time. However, since some topcis can be pretty difficult to examine here, I decided to look at them sepereately as well.



![too big](/images/NLP/female_entrepreneurship.png)


Female entrepreneurship was a topic I didn't even think about until the NMF model presented it to me. And still, I figured the topic would have an upward trend. To my chagrin, it almost seemed like a downward slope. There was a spike in 1977! Why would have this been?

To check the legitmacy of my model, I did some research, and looked into the articles from that time. Lo and behold, this is when Carter became President. As a proponet of female entrepreneurship, he signed an executive order to expand women's business enterprises. Thus the small business administration was providing resources to help women small business owners. And there were plenty of articles on this topic!

![too big](/images/NLP/health_insurance.png)

Here too the chart looked interesting. I wanted to check out why there was a spike between 1991 and 1994. And after some research, I found out that it was because there was talk to pass universal health care. And as such, there was a bunch of commotion around the issue.


![too big](/images/NLP/social_media.png)


![too big](/images/NLP/ride_sharing.png)


These charts generally make sense, and generally look like what I would expect.


I also wanted to look at general sentiment over time. Using Vader sentiment analysis, I analyzed each article. Overall, I noticed that most of the articles had positive senitment. As a matter of fact, there were twice as many articles that were tagged with a positive sentiment, than either netual or negative articles.

I tried plotting sentiment over the years, but there was just too much noises, and I didn't get back meaningful information. Basically looks cyclical.


![too big](/images/NLP/general_sent.png)



So I didn't really uncover any major secrets, but it was really cool how my data was about to illuminate all these stories, when all I gave it were a bunch of articles! 


Next steps would to be decompose the general business topic even further. This could be by running a topic analysis on only the articles classified with the general_business topic or to classify articles by the second highest topic probability.


The t-SNE really does suggest that there is more separation.

