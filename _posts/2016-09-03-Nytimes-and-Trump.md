## Trump's Presidential Campaign and the Media

When Donald Trump first entered the Republican presidential primary on June 16, 2015, no media outlet seemed to take him seriously as a contender. FiveThirtyEight, one of the most prominent political polling websites, said Trump ["has a better chance of cameoing in another “Home Alone” movie with Macaulay Culkin — or playing in the NBA Finals — than winning the Republican nomination.”] (http://fivethirtyeight.com/datalab/why-donald-trump-isnt-a-real-candidate-in-one-chart/). Politico considered his speech a [“bizarre spectacle”](http://www.politico.com/story/2015/06/donald-trump-2016-announcement-10-best-lines-119066) that was one of the most entertaining ones in the 2016 political season so far. NYTimes wrote about ["suspicions that he is entering the race mainly to appear in debates and win attention from the media.”](http://www.nytimes.com/2015/06/17/us/politics/donald-trump-runs-for-president-this-time-for-real-he-says.html?_r=0)
 
By any measure Donald Trump is an unusual presidential candidate. If elected, he would be one of the few presidents who had never held elected office, and the only one who had not served in the military or been appointed to political office instead. Before his run, he was best known to many Americans as a reality television star on his series “The Apprentice.” In the primary, many career politicians and political observers expressed disbelief at his belief to say many controversial and inflammatory statements that could end the of the race for another candidate.

Some in the media have admitted that they, and the media more generally, don't know how to cover him, both in the [primary](http://mashable.com/2015/12/02/the-media-just-doesnt-know-what-to-do-with-donald-trump/#RpvubaWmO8q3) and now in the [general election](https://thedianerehmshow.org/shows/2016-08-11/challenges-for-the-media-in-covering-donald-trump). Trump himself has criticized the media’s coverage of him:

![center](http://robinsones.github.io/images/Trump_media_tweet.png)

So far, this discussion of the media and Trump has mostly been anecdotal. I was interested in quantitatively examining how the media has covered Trump since the start of his campaign until now. For my initial exploration, I focused on extracting the topics of the NYTimes' articles covering Trump. You can find all of my code and my presentation to Metis on [my github repo](https://github.com/robinsones/NYTimes-and-Trump).

## Creating the dataset

The NYtimes has a pretty [user-friendly API](https://developer.nytimes.com). You simply need to request an API key and then you can get started gathering articles.

Because you can only get up to 1000 articles for a search term in a given time period, I broke my search down into all two week periods between the start of Trump’s campaign (June 16, 2015) until now. I then faced the main limitation of the NYTimes API: it doesn’t return the full-text of the articles. For this, I turned to the [newspaper package for Python](https://github.com/codelucas/newspaper).

I use Python 2, so I was stuck using the self-proclaimed “deprecated and buggy” Python2 branch of the package. As promised, it was difficult to install, but once I got it working it went very well. The newspaper package takes a url and then returns information about the article, including the authors and full text. The package also includes some natural language processing and can give you a summary and keywords list for each article.

I fed the urls returned by the NYTimes API into newspaper’s functions and quickly had the full text of some of the articles. I say some, because the NYTimes API actually returns a lot of dead links, which I decided to ignore. In the end I had about 2,200 news articles on Trump.

The next step was to limit my articles that were primarily about Trump. Because the API returned any article that simply had “Donald Trump” in it, I had a lot of irrelevant articles that were not actually about Trump and would have biased the analysis. I used the previously mentioned keywords function in the newspaper package to find the keywords for every article. I then selected only those articles that had “Donald” or “Trump” as a keyword.

## Topic Modeling

To extract the topics of articles, I first had to transform each article into a vector.  I did this using tf-idf, short for "term frequency-inverse document frequency." Tf-idf is a statistic for each word in an article that increases as the number of times a word appears in that article, but is offset by how frequently that word appears across the corpus of documents. In this case, the corpus was the set of all the articles about Trump. When I used tfidfvectorizer to transform the corpus of documents, I get a matrix where each row is an article and each column is a word that has appeared in the corpus. Tf-idf is useful for when all your documents are about the same broad subject and likely to have a lot of words in common. 

There are a few tricks to using tfidfvectorizer. The first is to remove a list of "stop words," or common terms such as "it" and "they" that are not informative about the content of a document. There is a built-in list of stopwords in sklearn, but I added my own words to this list after I ran my first topic model and found a lot of irrelevant words (such as "mrs" and "nytimes"). The second is to set a minimum and maximum document argument. These exclude words that appear in more documents that your maximum number or that appear in fewer documents than your minimum. This avoids words that are so rare or so common that they are unlikely to be meaningful for understanding what a specific article is about.

Once I had my transformed articles, I could then use Non-negative Matrix Factorization (NMF) to extract the topics of articles. LDA is the more common method for topic modeling but can't be used with tf-idf. When using NMF you have to pick a number of topics to extract. There's no "right" answer for how many or a statistic you can use to help you chose. In my case, I iterated though multiple different numbers and settled on 30. This was the tipping point where all the topics were interpretable but any further topics were not. 


