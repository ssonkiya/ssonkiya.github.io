When I was working at Etsy, I benefited from a very robust A/B testing system. Etsy had been doing A/B testing for more than 6 years (see a great and still relevant 2012 presentation from Dan McKinley, former Principal Engineer, on designing for [continuous experimentation](http://mcfunley.com/design-for-continuous-experimentation). By the time I left, Etsy's in-house experimentation system, called Catapult, had more than 5 engineers working on it fulltime. Every morning, I was greeted with a homepage that listed all the experiments that had ever run at Etsy. When you clicked on one, you got a summary of the experiment (usually by the product manager) and all the key metrics, such as conversion rate, add to cart rate, and number of visitors in each condition were already calculated. You could easily add any event that happened on the site and get those rates calculated too. You got to see the % change from the control to treatment with it's accompanying p-value, how many days until we had 80% power to detect a change. We even had beautiful little confidence intervals that changed color based on whether they overlapped with zero! 

And yet sometimes I would spend a majority of my time working on experiments, even though I tried to never be working on more than 3 or 4 at once. 

The concept of A/B Testing is deceptively simple. A classic example is you change the color of a button and measuring if the click-rate changes. Assuming your assignment of visitors and data collection is working collection, all you need to do is run a proportion test, right? 

### Basic A/B Testing Rules

1) Have one key metric for your experiment (e.g. click through rate, conversion rate, etc). You can monitor more metrics to make sure you don’t accidentally tank them, but you should have one as a goal. Revenue is probably the **wrong** metric to pick. It is likely a very skewed distribution which makes traditional statistics tests behave poorly. See my discussion in [my A/B testing talk](https://www.youtube.com/watch?v=SF-ryGgLOgQ) (around the 23 minute mark). 

2) Use that key metric do a power calculation. You'll input what the current rate of that metric is, how many visitors you get daily, what type of change you’re aiming to get (1% increase? 5%?), the percentage of people you’ll be allocating to your new version (e.g. are you doing 50/50 or 75/25 split), desired level of power (usually 80%) and the significance threshold (usually .05), and you can get how long you need to run this experiment for. If you’re doing a proportion metric, [experimentcalculator.com](experimentcalculator.com) is good for this. 

Two things will generally happen: 1) it will take a few weeks or 2) it will take 10 months. If the latter happens, you may either have to go for a different metric with a higher baseline rate or decide you only care about bigger changes (e.g. “I’m ok that we can’t measure if we increase clicks by 1% because only a 10% or greater increase is meaningful).

3) Run your experiment for the length you’ve planned on. Monitor it in the first few days to make sure nothing exploded. But plan on running it for the length you said. Don’t stop as soon as something is significant or you will get a lot of false positives. See the review section in Dave Robinson's Bayesian A/B Testing [blog post](http://varianceexplained.org/r/bayesian-ab-testing/). 

4) Confidence intervals are more important than the p-value. They have a 1-1 relationship such that if the p-value is less than .05, the 95% confidence interval does not overlap with 0. But if the confidence interval is wide and very close to zero, you’ve got a lot less evidence of a change than if it’s tiny and far away. 

5) Don't run tons of variants. You will lower your ability to detect a statistical effect and raise the likelihood of a false positive. 

6) Don't try to look for differences for every possible segment (e.g. did we help US visitors? new visitors? visitors on Saturday?). You'll again have false positive problems. You also probably won't be powered to detect changes either. 

7) Check that there's not bucketing skew. 

8) Don't overcomplicate your methods. Maybe you have engineers who've read about [multi-armed bandit testing](http://stevehanov.ca/blog/index.php?id=132), stats nerds who want to use Bayesian methods, or product managers who want the key metric to be a complicated sequence of behaviors. 

9) Be careful of launching things just because they "don't hurt." Negative change may be too small to detect. 

### Resources

When I started at Etsy, I read a lot of papers and blog posts on A/B Testing. While I had strong background in statistics and experimentation, A/B Testing presents an additional set of problems. These are some of the ones that I found most helpful.  

#### Getting Started
- [Democraticizing online controlled experiments at Booking.com](https://arxiv.org/pdf/1710.08217.pdf): booking.com is definitely #goals of experimentation (they run more than 1,000 concurrent experiments at a time and have a team of 30), but this overview of their experimentation system provides you a great place to strive for. While you certainly won't be able to replicate everything they do, such as have two separate pipelines that compute experiment metrics, one in real-time and only daily, you can learn from their emphasis on building trust in the infrastructure and data quality, maintaining a pool of A/A experiments (where there's no difference) to surface issues, enforcing good practices of pre-registration and statistical checks.
- [From Infrastructure to Culture: A/B Testing Challenges in Large Scale Social Networks](https://content.linkedin.com/content/dam/engineering/site-assets/pdfs/ABTestingSocialNetwork_share.pdf): while the first section deals with the complications of overlapping experiments and social networks, the latter sections have great rules of thumbs for simplifying the multiple testing problem and allowing metric owners to follow other people's experiments that are impacting their metric. 
- [Online Experimentation at Microsoft](http://ai.stanford.edu/~ronnyk/ExPThinkWeek2009Public.pdf): an accessible overview of how Microsoft started A/B Testing, tackling the issues of how most experiments fail, how they spread awareness, and lessons learned. 
- [Controlled Experiments on the Web: Survey and Practical Guide](https://ai.stanford.edu/~ronnyk/2009controlledExperimentsOnTheWebSurvey.pdf): an essential guide to best practices, some of which are covered in my rules above. 

#### Pitfalls
