---
title: Sentiment Analysis
permalink: Small-Businesses-Sentiment-Analysis
---

For this post, I further experiment with sentiment analysis using the small business text data I previously obtained [here](https://ssonkiya.github.io/Small-Businesses-Make-it-Big).

There are primarily two popular “out of the box” python packages that will conduct sentiment analysis. The first is [TextBlob](https://textblob.readthedocs.io/en/dev/quickstart.html), and the other is [vaderSentiment](https://github.com/cjhutto/vaderSentiment). Testing both, I realized that vaderSentiment yielded more accurate results (and ones I agreed with) than those from TextBlob. vaderSentiment, per their readme, “is specifically attuned to sentiments expressed in social media.” Not exactly my use case, but since it definitely performed better than TextBlob, I chose it as my sentiment analyzer.


Inspired by this [article](https://insight.factset.com/resources/at-a-glance-alexandria-text-analytics-economic-index-datafeed) from Factset on a sentiment solution they’re offering, I figured I would try to see if I could do something similar with the sentiment data I already had. 

I wanted to study whether sentiment analysis has any predictive power in tracking small business economic performance. I decided I would use the iShares Russell 2000 ETF (ticker IWM) as a proxy to measure small business economic performance. “IWM” tracks 2,000 of the smallest stocks in the US equity market based on market capitalization. Although the usual article content is designed to target private businesses that are relatively smaller than those traded in the US stock market, I decided this ETF would be the closest proxy for small business performance, which also contained robust enough volume trading data to validate the models employed. 

As mentioned in my previous post, the majority of my articles contained positive sentiment, which either means that, small business articles were actually generally positive, or that vaderSentiment wasn't doing a good job.

The sentiment data frame aggregated by day looked like the following:

![too big](/NLP_sentiment_analysis/code0.png)

Below I pull in stock data and combine with sentiment data:

![too big](/NLP_sentiment_analysis/code1.png)


Next, I calculated zscores to scale my data for comparison.

![too big](/NLP_sentiment_analysis/code2.png)


Looking at the time series plot below, generally the sentiment data looks noisy. You can definitely see the sentiment fell around late 2008, along with the stock prices, which is what we would expect. Yet if sentiment had significant predictive power, we would expect to see a fall in sentiment preceding a fall in market prices. However, it is interesting to note that sentiment does rise more quickly than the stock market prices coming out of the financial crisis.


![too big](/NLP_sentiment_analysis/graph.png)


I considered filtering to articles with only strong distinct sentiments (articles closer to neutral would be dropped), but that was going to drop almost two-thirds of my data.

This speaks to why it is necessary to have sentiment algorithms that are trained and vetted specifically on financial news. Out of the box open source packages just don’t seem to work very well for this kind of analysis. 


