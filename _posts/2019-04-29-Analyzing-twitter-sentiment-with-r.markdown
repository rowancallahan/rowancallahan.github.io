---
layout: post
title:  "Analyzing Tweets and Sentiment with R"
date:   2019-04-29 18:08:46 -0400
categories: jekyll update
---

**Analyzing Twitter Sentiment**

A while back I was talking to a friend about their experience using Twitter and the twitter API. I had always assumed that it must have
entailed a lot of web scraping difficult parsing and headaches.

However, I was pleasently surprised to learn that this in fact never was the case. Its so easy that there are a plethora of ways to get started
and a number of R packages that do most of the heavy lifting for you.

One of the most difficult parts of the entire experience turned out being registering my username with twitter and waiting for them to grant me api access.

A few helpful blog posts were available to provide some pointers for setting up my api key registration [here][registration] and an incredibly user friendly API
that helped me to get started parsing and downloading tweets. [here][link1]

For a list of positive and negative sentiment words, I used the list created by Liu Bing at the university of Illinois at Chicago(UIC), [here][link2]


**Finished product**
You can find the finished project as an R shiny app [here][app].

![appexample](/assets/rshiny_app_pic.png){:class="img-responsive"}


There were a few things that I was surprised by while making this app:


* getting color scales to work on plotly is incredibly difficult and getting a flipped colorscale that is correct can be a non trivial endeavor.
* Most tweets do **not** have location data, so mapping out twitter trends is much more difficult than people on line make it seem
* R has a very mature way of dealing with tweets and processing twitter data with multiple walkthroughs and packages.




[link1]: [https://www.rdocumentation.org/packages/twitteR/versions/1.1.9]
[link2]: [http://www.cs.uic.edu/~liub/FBS/opinion-lexicon-English.rar]
[registration]: [https://chimpgroup.com/knowledgebase/twitter-api-keys/]
[app]: [http://rlccodeexamples.shinyapps.io/twitter_opinions_example/]
