---
title: When Small Businesses Make it Big
permalink: Small-Businesses-Make-it-Big
---

For my unsupervised Natural Language Processing (NLP) project, I decided to take a look at trends in small business topics over a span of almost 50 years, through the lens of New York Times.


With a relatively easy to use API, and access to a multitude of articles across decades, New York Times was the obvious newspaper of choice for the project. I filtered for articles that matched on keywords of "Entrepreneurship," "Small Business," and "Venture Capitalism," casting a net wide enough for multiple matches, yet still constraining only to articles that would encapsulate the idea of small business and entrepreneurship.


Unfortunately, and also, kind of surprisingly, it was incredibly difficult to match on a very large number of articles - in total I parsed through 8,000 articles from 1970 to current. Of these, there were articles that, though might have been written for entrepreneurs, were briefings and overviews of global events. Since macro related topics were out of scope for the project, I further cleaned my corpus my dropping articles of such content. This finally led to a working clean corpus of about 5,500 articles.


Article distribution by year is reflected in the chart below. It is apparent that more articles with entrepreneurship tags were published in the 2000s than in prior years.

![too big](/images/NLP/article_dist.png)

I preprocessed the NYT corpus using standard NLP methods (NLTK tokenization, cleaning data for punctuation, numbers, and stop words, and TF-IDF with max_df of 0.4).

Running an NMF model yielded the following topics:

![too big](/images/NLP/topic_modelling.png)


The topic delineation looked fantastic, and topics were generally easy to identify from the associated words. There was a general catch all topic which I called the "general_business" topic, suggesting further delineation between topics could have been possible.


I didn't want to classify each newspaper article to a specific topic, which is often done by using max topic weight.
Instead I looked at the mean percent of topic for each article. When grouped in years, this allowed years with a smaller number of articles to still have a decent spread of topics.

An overview of mean percent topic change over time is below.
![too big](/images/NLP/tableau_topic_percent.png)


This illustrates that in 2015-2018, on average ~30% of articles were about general business, and 16% of the articles were related to the Silicon Valley topic, etc. 


Since some topics can be pretty difficult to examine here, I decided to look at them separately as well.

![too big](/images/NLP/female_entrepreneurship.png)

Female entrepreneurship as a topic didn't cross my mind (unfortunately) until I was presented with it via the NMF modelling (wonders of unsupervised learning!). And even then, I assumed that it would be an upward trending topic.
To my chagrin, the graph has an almost downward trending chart. Why was there such a spike in 1977?


To check the legitimacy of my model, I did some research, and looked into the articles from that time. Lo and behold, this is when Jimmy Carter became president. As a large proponent of female entrepreneurship, he signed an executive order to support the expansion of women lead enterprises. Though the executive order was signed in 1979, work towards that goal was well under way by 1977.

![too big](/images/NLP/health_insurance.png)

I found the above graph to be interesting as well. I wasn't sure why there was a spike between 1991 and 1994. After some research, I found out that it was due to talk surrounding universal health care legislation. And as such, there was a bunch of commotion around the issue.


![too big](/images/NLP/social_media.png)


![too big](/images/NLP/ride_sharing.png)

These charts generally made sense, and looked within expectations.


Lastly, I wanted to take a look at general sentiment. Using polarity weights from vaderSentiment, I classified each article to generally have positive, negative, or neutral sentiment. Overall, I noticed that most of the articles were classified to be positive. As a matter of fact, there were twice as many articles that were classified to be positive, than either negative or neutral.

> Expanded upon the sentiment analysis [here](https://ssonkiya.github.io/Small-Businesses-Sentiment-Analysis)

![too big](/images/NLP/general_sent.png)


Though I didn't really discover any new ground, I was amazed at how well NLP was able to illuminate such interesting stories given just a corpus of newspaper articles.


The next step would to be decompose the general business topic even further. This could be accomplished by running a topic analysis on only the articles classified with that topic or to classify articles by the second highest topic probability.



