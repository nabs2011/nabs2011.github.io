---
layout: post
title: Sentiment Prediction of GlobalGoals Tweets
---

The Social Good Summit is held annual to showcase how various NGOs, companies and governmental bodies are taking steps to meet the sustainability goals (aka Global Goals) outlined by the UN. The main summit, held in New York, generates a considerable amount of discussion on Twitter. I wanted to examine the sentiment and words used in tweets associated with the summit. Throughout the week of 19 September 2016, the week of the event, I harvested 11,573 tweets that used the key phrase 'globalgoals'. I stored this collection of tweets using a PostgreSQL database, which I then accessed and analysed in a Jupyter notebook. 

To analyze sentiment I went through the following process:

- Calculate sentiment score for each tweet using nltk.corpus.sentiwordnet
- Created a word count matrix as my feature set
- Use RANSAC and ElasticNet to predict sentiment score  

In conclusion, I found that ElasticNet generated predictions capture the dynamicity of word-based sentiment amongst the tweets. It will also explain around 94% of the actual results. RANSAC-based prediction will tend to overestimate sentiment results.
![_config.yml]({{ site.baseurl }}/images/globalgoals.png)

_Image: RANSAC predicitions are in blue and ElasticNet predicitions are in red._


Possible next steps

* Explore the negative and positive peaks within the distribution of sentiment.
* Build a model that includes performance data (e.g. retweets) to understand how sentiment impacts performance.
* Develop a way to identify sentiment-ambiguous words, which can be used to improve the accuracy of sentiment-scoring.

Click [here](https://github.com/ByronAllen/Portfolio/blob/master/Project_NLP_GlobalGoals_Twitter.ipynb) to see the full notebook.

