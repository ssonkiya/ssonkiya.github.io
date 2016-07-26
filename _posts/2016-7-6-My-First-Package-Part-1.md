A summary of my experience with STATA in graduate school:
* Googling my problems and being directed to [old](http://www.stata.com/statalist/archive/2010-04/msg01673.html) [listserv](http://www.stata.com/statalist/archive/2009-04/msg00976.html) [posts](http://www.stata.com/statalist/archive/2007-09/msg00099.html) insead of the [beautiful](http://stackoverflow.com/questions/20987295/rename-multiple-columns-by-names) [Stack Overflow](http://stackoverflow.com/questions/12357592/efficient-multiplication-of-columns-in-a-data-frame) [answers](http://stackoverflow.com/questions/4203442/for-loop-vs-while-loop-in-r) I had been spoiled with when using R. 
* Wishing there was a way to create reproducible documents with code, output, and explanations as I did with R Markdown and Jupyter Notebooks
* Yelling at the computer “I know how to do this in R!" and "This is better/easier/prettier in R!"

But STATA was the language of INSEAD's analytical classes, and so STATA was the language I would use. Until I finally reached a breaking point while trying to replicate the [MEMORE](http://afhayes.com/spss-sas-and-mplus-macros-and-code.html) macro in STATA for a spontoneous class assignment by a teacher who had not yet tried to do so himself.

## The MEMORE function
MEMORE stands for "MEdiation and MOderation in REpeated-measures designs" and is a macro for SPSS and SAS created by Montoya and Hayes. As they describe: 

> It estimates the total, direct, and indirect effects of X on Y through one or more mediators
M in the two-condition or two-occasion within-subjects/repeated measures design. In a pathanalytic
form using OLS regression as illustrated in Montoya and Hayes (2015), it implements the
method described by Judd, Kenny, and McClelland (2001, Psychological Methods) and extended
by Montoya and Hayes (2015) to multiple mediators. Along with an estimate of the indirect
effect(s), MEMORE generates confidence intervals for inference about the indirect effect(s) using
bootstrapping approach.

I managed to get the STATA code to work eventually, but in the meantime I decided to tackle it with R. I also wanted the chance to keep to continue practicing using R Markdown instead of R Scripts. With R Markdown, I could not only write the function equivalent to the MEMORE macro but also include an explanation of the function in the same document. You can see my original report [here](http://robinsones.github.io/files/MEMORE.pdf). Using a combination of the [documentation of the function](http://afhayes.com/public/memore.pdf) and [the accompanying paper](http://psycnet.apa.org/psycinfo/2016-32270-001/), I replicated the basic function and including the options for setting number of bootstrap represetitions, confidence interval (CI) size, and which bootstrap CI would be calculated. 

This did earn me some praise from my difficult-to-please German professor:

![](http://robinsones.github.io/images/Excellent_Feedback.png)

I also created a [GitHub repository](https://github.com/robinsones/R-MEMORE) to store my R Markdown document. But from March until a week ago I had pretty much left it there. 

## Creating a package

I had bookmarked [Hilary Parker's blog post](https://hilaryparker.com/2014/04/29/writing-an-r-package-from-scratch/) on writing R packages months ago, but never got around to doing anything with it. I appreciated how straightforward and simple she made the process seem (and of course that she used an example with cats). 

Since her post, Hadley had released a [book](http://r-pkgs.had.co.nz) on writing packages in R. I started reading the first chapter, but I quickly realized I would only really retain and understand writing packages if I did it myself. So I decided to finally turn the MEMORE R Markdown document into a real, easily useable package. 

