---
layout: post
title: Twitter and Word Cloud 
tags: [data, twitter, python] 
---

A Twitter bot, called [@wordnuvola](https://twitter.com/wordnuvola), attracted my curiosity: it can generate a word cloud of your tweets (nice!). As a user, you simply have to follow the account and send back a tweet. In return, you have a nice image after some minutes. The service seems quite successful (viral?) since many people I follow use it for fun. My word cloud result was:

![Extraction process](/assets/wordcloud-nuvola.png)

Two questions came to my mind:

* *Is the word cloud accurate?* I was expecting some keywords to be present and, for instance, I was surprised to have "splc18": SPLC is an [important conference](http://splc.net) in my research area, I've tweeted about the 2018 edition, but not that much. 
* *How the Twitter bot can know about (all) my tweets?* After all I did not give special access to my tweets and I have written thousands (most of the tweets are actually retweets).

To answer these two questions, I've decided to replicate the generation of the word cloud. It toke less than 1 hour, including the following steps:

* request and download the full archive of your tweets (in the Twitter preferences)
* understand how data/tweets are organized (it was a basic CSV file, where each row corresponds to a tweet)
* process the tweets and generate the word cloud. I used Python, pandas, and [wordcloud](https://amueller.github.io/word_cloud/) (actually created and maintained by one of the core contributor of scikit-learn, Andreas Mueller!)
* fine-tune and test the solution (see below)

To obtain an interesting word cloud, an important step is the pre-processing of data:

* removal of URLs or twitter's @usernames (otherwise your most frequent words are followers you exchange with)
* stop words: wordcloud library has a default set of words (english) to ignore, but as I tweet sometimes in French I need to add others… 
* consider or not tweets that are retweets (I prefer to not include them)



The final result is:

![Extraction process](/assets/wordcloud-my.png)

Clearly, the word cloud differs. So why? 

I experimented a bit: with or without RT, tweets after 2018, etc. My conclusion was that the bot only considers recent tweets and RTs. Am I right? 

Actually yes ;-) In fact the bot is open source and is publicly available [here](https://github.com/defacto133/twitter-wordcloud-bot/blob/master/twitterapi.py). The analysis of the source code shows that:

* 'include_rts': 'true'
* 3200 tweets are collected, for a maximum page of 16 (thanks to Twitter API)

So the general conclusion is: 

- *Is the word cloud accurate?* No since it considers only recent tweets. 
- *How the Twitter bot can know about (all) my tweets?*  It doesn't know all tweets, only a sample. 

Anyway, this service is fun and has the merit of being easy to use. But you know the secrets now ;) If you want a more comprehensive solution, I have shared [my code](https://github.com/acherm/wordcloud-tweets). 









*(new!) I realized that some of my blog post entries are sometimes cited in academic works, so why not using the following bibtex entry?*

```bibtex
@misc{acher2019twitterwordcloud,
  author = {Mathieu Acher},
  title = {Twitter and Word Cloud},
  year = {2019},
  month = {feb},
  howpublished = {\url{https://blog.mathieuacher.com/TwitterWordCloud/}},
  note = {\url{https://blog.mathieuacher.com/TwitterWordCloud/}}
}
```
