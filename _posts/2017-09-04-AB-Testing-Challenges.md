A few weeks ago, I wrote about[link] my experience giving my first data science talk. While you can find the talk available online[link], I wanted to share in writing some of the points I covered. I’ve also added a few new points on business challenges that I didn’t have time to cover in the talk. 

The talk and this post are divided into two main sections: statistical challenges and business challenges. One of the statistical challenges I covered, developing a test for differences in Average Converting Browser Value between two groups, is going to be posted on Etsy’s engineering blog, Code as Craft. XYZ. 

Statistical Challenges
======

*Browser vs Visit: Level of Analysis*

Business Challenges
======

At Etsy, the analysts work with partner teams, including marketing, seller services, retention and loyalty, and finance, among others. I’ve been working with search since I’ve joined and have worked with three different teams in that time. Right now I work with the ranking team (all engineers) and with the Search Experience team, a more traditional team with engineers (including a lead engineer), Product Managers (PMs), and designers. What we help them with can vary a lot: while I mainly work on experimentation, other analysts create dashboards in Looker, new tables to make data access easier, or a model for estimating lifetime value of a new customer.

*No Feigning Surprise*

Many Etsy engineers come from the Recurse Center[link] (formerly known as Hacker School), a free, six or twelve week self-directed retreat for programmers. The Recurse Center has a set of social rules (along with a code of conduct) to make the environment productive and supportive for everyone. One of those is no “feigning surprise” when people admit they don’t know something. As they put it[https://www.recurse.com/manual#sub-sec-social-rules]: 

> Feigning surprise has absolutely no social or educational benefit: When people feign surprise, it's usually to make them feel better about themselves and others feel worse. And even when that's not the intention, it's almost always the effect. As you've probably already guessed, this rule is tightly coupled to our belief in the importance of people feeling comfortable saying "I don't know" and "I don't understand."
 
To be a successful partner, you need to make sure your team is comfortable asking you questions. Don’t make assumptions about what your audience knows; even though you live p-values day in and day out, they don’t. You should lean on the side of always defining terms, which also helps for when people think they know what something means but they actually don’t (common with p-value). 

*Understand your partner team*

Something that took me a little time – developing empathy. Experiments that fail are hard; Microsoft experiment team sometimes our job is telling them baby is ugly. Can have emotional or financial investment. 
Understand your team: their data literacy, their motivations, how often do they check experiments (likely answer: as often as they can) 

*Invest in Education Efforts*

Education efforts can and save you time . Your partner team has domain knowledge and some intuition. Can save you work if they know some stuff themselves.

At Etsy, we’ve been working on a multi-pronged approach to education. The data engineering team developed an experiment hub, where they’ve gathered information about our various tools. Another analyst and I wrote a general guide to experiments at Etsy, covering XYZ. We also gave an “Edsy,” an internal talk. I’m now experimenting with offering office hours for people to come with general questions about experimentation, such as “what is power?” or “XYZ.” We have a slack channel dedicated to experiment questions, which not only data analysts and engineers but also experiment savvy engineers monitor.  

 One company has taken this to the extreme: Airbnb, with a [Data University](https://medium.com/airbnb-engineering/how-airbnb-democratizes-data-science-with-data-university-3eccc71e073a).  

*Develop consistency among the Analyst Team*

Consistency among your analyst team is important. Something we’re working on here. Problem if teams think you’re being stricter than others, or if people are calculating incremental revenue/GMS differently. If one team says you have to run experiments for a week, even if you’re powered before then, while others say 4 days is fine, that’s an issue. Also can mean learning from each other. Finally can help when you switch analysts for a team; example, when I had to step in, team was like “what do you mean you can’t do app and web experiments in one go?” I hadn’t even thought that was possible with our system. 

*No Peeking*

*Lay out Hypotheses*

Lay out hypotheses ahead of time. Especially whether you will launch on neutral. No one thinks they need to do that because they think results will be really clear. Also, you’ll be surprised as the sheer combination of results that are possible (e.g. search clicks went down, but mean search purchase went up, and conversion is neutral, but add to cart is slightly down, etc.). 

Conclusion
======

You’ll notice that the business section was much longer than the statistical one. While this is partly because I cover one of the statistical challenges in another post, this also reflects XYZ. I think Yonatan Zunger, a former senior Google engineer, covers this very well in the second section of his [post]( https://medium.com/@yonatanzunger/so-about-this-googlers-manifesto-1e3773ed1788) on James Damore’s Google manifesto. While he is talking about engineering, I think his points can be equally applied to data science: 
  
> People who haven’t done engineering, or people who have done just the basics, sometimes think that what engineering looks like is sitting at your computer and hyper-optimizing an inner loop, or cleaning up a class API…Engineering is not the art of building devices; it’s the art of fixing problems…Essentially, engineering is all about cooperation, collaboration, and empathy for both your colleagues and your customers.

This can be why non-traditional backgrounds can be very successful in tech jobs – often people have learned this skill in their previous career. 

