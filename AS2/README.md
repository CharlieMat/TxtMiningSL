# Assignment #2

What are key factors that can affect the quality of results from a NER system?

## NER system

Python nltk: official document [here](http://www.nltk.org)

## Collecting data

Available tweets and news data are already in folder *./AS2data*, tweets data file begin with "captured_tweets_" and news data begin with "google_news_"

To get more data:

<pre><code> python as2getdata.py </code></pre>

This code will search tweets using [twitter streaming api](https://dev.twitter.com/streaming/overview) and search [google news](https://developers.google.com/news-search/) using [gnp](http://mpand.github.io/gnp/) package. The current topic is set to "Oscar". Data will be saved into folder *./AS2data*.

## Run data through NER system

To extract information from the data file through NER system:

<pre><code> python as2dataextract.py </code></pre>

The code extract those messages or sentences from tweets and google news data files retrieved from last step. It only extract 50 for tweets and 50 for google news. Then it uses nltk NER system with [Gutenberg corpus](https://pypi.python.org/pypi/Gutenberg/0.1.1) to tokenize those sentences and tagged those tokens by POS labels. Finally, it finds out all "PERSON" labelled tokens. For both tweets and google news data, there will be three files output from this step:

* *tweets_before_ner* / *news_before_ner* : list of sentences or messages
* *tweets_after_ner* / *news_after_ner* : NER results of all these sentences or messages
* *tweets_person* / *news_person* : list of PERSON tagged tokens in NER results

## Evaluate results

The results generated from the last step does not provide the true results. So I manually compared the results and their corresponding sentences. The record is already provided as file *./AS2data/tweets_analysis.json* and *./AS2data/news_analysis.json*.

Since I will use precision, recall and F-score as evaluation output, record of each sentence will contain three parts:

* "P": number of PERSON tokens that actually in a sentence
* "RP": number of PERSON tokens that correctly retrieved by the NER system
* "R": number of PERSON tokens that retrieved by the NER system

As a result, the precision score is defined as **RP/R**, the recall score is defined as **RP/P**, and the F-score is defined as **\(2\*precision\*recall\)/\(precision+recall\)**

To calculate these scores:

<pre><code> python as2analyse.py </code></pre>

The code will calculate:

* precision, recall and F-score for *tweets_analysis.json*
* precision, recall and F-score for *news_analysis.json*

## The result

|item		|tweets 		|news 				|
|-----------|---------------|-------------------|
|amount		|50 			|50 				|
|recall 	|0.615384615385 |0.915254237288 	|
|precision 	|0.484848484848 |0.701298701299 	|
|F-score 	|0.542372881356 |0.794117647059 	|

As expected, tweets are much more difficult than news sentences to analyse using nltk NER system. The reason may include the length and carefulness of text.

## Conclusion

Though this experiment is incomprehensive, it reveals that the type of data(tweets or news sentences in this project) is critical issue affects the quality of NER results. And in turn, I can also say that the target data type of the NER system. Like in this project, nltk tools and gutenberg corpus are intended to deal with formal text that is more well-written, so it is quite possible that it performs much worse for tweets.