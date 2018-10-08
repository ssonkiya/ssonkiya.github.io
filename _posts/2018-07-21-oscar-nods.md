---
layout: post
title: Osca-regression
permalink: /oscar-nods/
---

The second project at Metis focused on web scraping and regression, and came with the suggestion to work with movie data. I suppose I'm more motivated by prestige than money because I had a :bulb: moment - could I predict the oscars? 

I had heard that studios will release their oscar contenders later in the calendar year, in order to stay fresh in the minds of the Academy. This makes sense, since nomination season begins in January, with the big shin-dig going down in late February. **I wondered, how does release day impact oscar nominations?**

I chose to scrape the top 300 films from 1980 onwards from Box Office Mojo, and ended up with the following information for each film; title, domestic total gross, run time, release date, director, production budget, length of time in release (number of days the film was out in theaters), mpaa rating, distributor, genre, number of oscar nominations, and number of wins. 

The first thing I noticed was that there are definitely some times of the year that see a greater number of releases. There a couple of steady lil bumps around the spring (day 100 is in mid-April), then in the fall (day 275 falls right at the beginning of October), and there is a steady increase leading to a huge spike on day 359 (Christmas Day.)

![releases per day](/images/oscar_nods/releases-doy.png)

This isn't that new to most of us - we know that there are typically a lot of films out in the summer, when schools out, and then also around the holiday season in the winter. So how do nominations stack up in this paradigm?

![nominations per release day](/images/oscar_nods/release-day-noms.png)

This tells us that there is definitely something to this whole later-in-the-year idea... Even though we can see films released earlier in the year have a decent shot at _one_ nomination, **a later release day is helpful in getting _more than one_ nomination.**

### Predicting Nominations

Before modeling and attempting to predict the number of nominations, there were a few things I had to consider. The first is that most of the films in the dataset didn't receive any nominations at all - only 16% received one or more nod from the academy. Also, nominations follow a poisson, or count , distribution; for all those situations you can't have a negative number, or half of one.  These things will certainly make things difficult for a regression problem. 

I use python, so ended up turning to the StatsModels library for the modeling phase, since SciKit Learn doesn't have poisson options in their linear models package. I'm curious about the role that release day plays, so I used the standard scaler to scale my features in order to compare the magnitude of the coefficients. In the end, I settled on a simple ordinary least squares model; the poisson regression in statsmodels _didn't_ give me whole numbers, and had a low r-squared metric. The two gave me pretty similar predictions as well, and in the end, **sometimes a simple approach is better**.

Surprisingly (or perhaps not surprisingly?), release day fell to the wayside compared to the :moneybag: , run time and number of days in theaters, while production budget had very little impact.

Feature| Coefficient
Domestic Total Gross |1.3798
Opening Gross| -1.2498
Run Time |0.4649
Days In Release |0.4001
Opening Ratio |0.1709
**Release Day** |**0.1358**
Opening Rank| 0.0276
Production Budget |5.817e-05

However, I do think that it is important to take these findings with a grain of salt. My model did tend to under predict nominations since there were so many films that didn't receive any nomination at all. My r-squared was .35 - not so great but not so bad considering what I'm attempting to predict. The graph below has my predictions along the x axis and the actual values along the y.

![preds vs actuals](/images/oscar_nods/ols.png)

### The Takeaways

I do think that this sort of problem may be better suited to classification techniques, which I'm excited to get into in our next module. Following that, we begin studying NLP, which I think would be a great way to capture features such as sentiment from media buzz and critic reviews - this may be able to better predict how much recognition a film may receive, since it's hard to capture such a visual art form with numbers alone.