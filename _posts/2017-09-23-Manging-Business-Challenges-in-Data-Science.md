A few weeks ago, I [wrote about](https://robinsones.github.io/Giving-Your-First-Data-Science-Talk/) my experience giving my first data science talk. While you can find the talk available [online](https://www.youtube.com/watch?v=SF-ryGgLOgQ), I wanted to share in writing some of the points I covered about managing business challenges, along with a few new ones. Additionally, one of the statistical challenges I covered in my talk, developing a test for differences in Average Converting Browser Value between groups, is going to be posted on Etsy’s engineering blog, [Code as Craft](https://codeascraft.com/archive/).

Managing Business Challenges
======

At Etsy, each of the analysts works with a partner teams, including marketing, seller services, retention and loyalty, and finance, among others. What we help them with can vary a lot: while I mainly work on experimentation, other analysts create dashboards in Looker, new tables to make data access easier, or a model for estimating the lifetime value of a new customer. Since starting at Etsy nine months ago, I've worked in search, partnering three different teams. Right now I work with our Ranking team (all engineers) and with our Search Experience and Personalization team, a more traditional team with engineers (including an engineering manager and lead engineer), Product Managers (PMs), and designers. 

Even if their role isn’t as deeply embedded with partner teams as ours, all data scientists and analysts (or their managers) will have to work with non-analysts<sup>1</sup> sometimes. While I still have a long way to go, I wanted to share some advice and techniques I've found helpful.  

**Don't Feign Surprise**

To be a successful partner, you need to make sure your team is comfortable asking you questions. One way to almost guarantee that they won't be is if you "feign surprise."

Many Etsy engineers come from the [Recurse Center](https://www.recurse.com) (formerly known as Hacker School), a free, six or twelve week self-directed retreat for programmers. The Recurse Center has a set of social rules (along with a code of conduct) to make the environment productive and supportive for everyone. One of those is no “feigning surprise” when people admit they don’t know something. As they [put it](https://www.recurse.com/manual#sub-sec-social-rules): 

> Feigning surprise has absolutely no social or educational benefit: When people feign surprise, it's usually to make them feel better about themselves and others feel worse. And even when that's not the intention, it's almost always the effect. As you've probably already guessed, this rule is tightly coupled to our belief in the importance of people feeling comfortable saying "I don't know" and "I don't understand."
 
You also shouldn't make assumptions about what your audience knows. Even if you live p-values and confidence intervals day in and day out, they don’t. You should lean on the side of always defining terms, which will also help in the case people think they know what something means but they actually don’t (very common with p-values).

**Develop empathy**

The microsoft experimentation platform team put it best [when they said](http://notes.stephenholiday.com/Five-Puzzling-Outcomes.pdf): "our job ... is to tell our clients that their new baby is ugly." As they discuss, while teams build new features or products because they believe they'll be useful, most experiments testing those new features reveal they don't have a positive impact. Teams are often emotionally invested in their new feature: not only have they spent a lot of time working on it, but also often it was one of the team members who came up with the idea in the first place. There may also be financial investment if a company has tied bonuses or performance reviews to experiment success. 

Even though I knew this intellectually, it took me time to develop empathy, and it's still something I'm working on. It can be frustrating when you have to correct misinterpretations of seemingly "positive" results from peeking (see section below) or [slicing data in every possible way](http://www.slate.com/articles/health_and_science/science/2013/07/statistics_and_psychology_multiple_comparisons_give_spurious_results.html). It's useful to remember to take a step back and see that you have the same overarching goal: for your team do well and thus help the company. 

**Understand your partner team**

Understand your team: their data literacy, their motivations, how often do they check experiments (likely answer: as often as they can) 

**Invest in Education Efforts**

While every company can't be Airbnb and have its own [Data University](https://medium.com/airbnb-engineering/how-airbnb-democratizes-data-science-with-data-university-3eccc71e073a), we should all be thinking about how we can help non-analysts understand and make data-driven decisions. First, it will save you time. If an engineer knows they'll need to record the listings that are eligible for a new badge in the variant **and** the control, so you can compare clickthrough rates, or the product manager knows how to use experimentcalculator.com and discovers their experiment would take 150 days to be powered, that's one less question they'll have to ask you.  

But secondly, your partner team has deep domain knowledge. If they can start thinking with a data mindset, they XYZ. 

At Etsy, we’ve been working on a multi-pronged approach to education. The data engineering team developed an experiment hub, where they’ve gathered documentation about our various tools. Another analyst and I wrote a general guide to experiments at Etsy, covering planning experiments to making launch decisions, and also gave an “Edsy,” an internal talk, on it. I’m now experimenting with offering office hours for people to come with general questions about experimentation. Finally, we have a slack channel dedicated to experiment questions, which not only data analysts and engineers but also experiment savvy engineers monitor. 

**Develop consistency among the Analyst Team**

Working with partner teams becomes much easier if you have some consistent standards and practices within the analyst team. For examples, here are some differences that can cause problems: 

- if your team thinks you're stricter than other analysts in judging experiment results 
- if analysts are calculating incremental revenue from experiment launches differently 
- if one analyst says you have to run experiments for at least a week, even if you're powered before than, while others say four days is fine

Working to develop consistency is also a great opportunity to learn for each other; for example, some analysts may not be as familiar with the problems of peeking and that's why they weren't as strict on not doing so. It can also help make the transition when a partner team has to change analysts much smoother.  

**No Peeking**

**Lay out Hypotheses**

Lay out hypotheses ahead of time, especially about whether you will launch on neutral. People often think they don't need to have specific hypotheses beyond "we'll launch if it's better" because they think results will be really clear. But you’ll be surprised as the sheer combination of results that are possible (e.g. search clicks went down, but mean search purchase went up, and conversion is neutral, but add to cart is slightly down, etc.). Picking one or two key metrics for launch and a few other metrics for monitoring will also help you from having a multiple comparisons problem, where you're [testing so many metrics one of them will be significant](https://en.wikipedia.org/wiki/Multiple_comparisons_problem), even if there really is no change. 

Conclusion
======

Why devote a whole post and half a talk to business challenges instead of cutting edge deep learning papers? While basic technical skills are the table-stakes for a data science career, the importance of non-technical skills in a data science career is often overlooked. I think Yonatan Zunger, a former senior Google engineer, covers this very well in the second section of this [post]( https://medium.com/@yonatanzunger/so-about-this-googlers-manifesto-1e3773ed1788). While he is talking about engineering, his points can be equally applied to data science: 
  
> People who haven’t done engineering, or people who have done just the basics, sometimes think that what engineering looks like is sitting at your computer and hyper-optimizing an inner loop, or cleaning up a class API … Engineering is not the art of building devices; it’s the art of fixing problems … Essentially, engineering is all about cooperation, collaboration, and empathy for both your colleagues and your customers.

I'll continue covering both business and statistical challenges in my next post, where I'll share what I've learned from reading papers on A/B testing in industry. 

1. I'm using "non-analyst" as a catch-all phrase for anyone who isn't in analytics (e.g. not a data scientist, business analyst, data analyst, etc.). 
