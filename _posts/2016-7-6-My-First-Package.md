A summary of my experience with STATA in graduate school :
* Yelling at the computer “I know how to do this in R!" and "This is better/easier/prettier in R!"
* Googling my problems and being directed to [old](http://www.stata.com/statalist/archive/2010-04/msg01673.html) [listserv](http://www.stata.com/statalist/archive/2009-04/msg00976.html) [posts](http://www.stata.com/statalist/archive/2007-09/msg00099.html) insead of the [beautiful](http://stackoverflow.com/questions/20987295/rename-multiple-columns-by-names) [Stack Overflow](http://stackoverflow.com/questions/12357592/efficient-multiplication-of-columns-in-a-data-frame) [answers](http://stackoverflow.com/questions/4203442/for-loop-vs-while-loop-in-r) I had been spoiled with when using R. 
* Wishing there was a way to create reproducible documents with code, output, and explanations as I did with R Markdown and Jupyter Notebooks

But STATA was the language of INSEAD's analytical classes, and so STATA was the language I would use. Until I finally reached a breaking point while trying to replicate the [MEMORE](http://afhayes.com/spss-sas-and-mplus-macros-and-code.html) macro in STATA for a spontoneous assignment by the teacher (who had not tried to do so himself).

MEMORE stands for "MEdiation and MOderation in REpeated-measures designs" and is a macro for SPSS and SAS created by Montoya and Hayes. As they describe: 

> It estimates the total, direct, and indirect effects of X on Y through one or more mediators
M in the two-condition or two-occasion within-subjects/repeated measures design. In a pathanalytic
form using OLS regression as illustrated in Montoya and Hayes (2015), it implements the
method described by Judd, Kenny, and McClelland (2001, Psychological Methods) and extended
by Montoya and Hayes (2015) to multiple mediators. Along with an estimate of the indirect
effect(s), MEMORE generates confidence intervals for inference about the indirect effect(s) using
bootstrapping approach.

I managed to get the STATA code to work eventually, but in the meantime I decided to tackle it with R. I also wanted the chance to keep practicing using R Markdown instead of R Scripts. This way I could not only write a function equivalent to the MEMORE macro but also include the explanation of the function in the same document. You can see my original report [here](/files/MEMORE.pdf). 

This did earn me some praise from my difficult-to-please German professor:

![](http://robinsones.github.io/images/Feedback_Excellent.png)
