---
title: Reposts of jokes on reddit.com/r/jokes
header:
  overlay_image: assets/images/r_jokes/snoopng_cropped.png
toc: true
author: Abhilash Sarwade
excerpt: Evaluation of number of reposts using TF-IDF 
---

# Objective

To quantitatively find out what fraction of jokes submitted on [reddit.com/r/jokes](https://www.reddit.com/r/Jokes/) are reposts.

# Introduction

**Reddit** is an American social news aggregation, web content rating, and discussion website where registered members submit content to the site such as links, text posts, and images, which are then voted up or down by other members. Posts are organized by subject into user-created boards called "subreddits", which cover a variety of topics including news, science, movies, video games, music, books, fitness, food, and image-sharing.

One of the popular subreddit is [r/jokes](https://www.reddit.com/r/Jokes/) with 17.6 million "humorists"  subscribed. It is currently 19th largest subreddit with about 1500 jokes submitted per month (greater than 50 upvotes). 

{% include figure image_path="/assets/images/r_jokes/no_jokes_month.png" alt="n_jokes_month" caption="Activity on r/jokes since the subreddit's birth." %}

# Reposting Problem

Though r/jokes is very large subreddit, it lacks the originality in submitted jokes. One of the running gags on the subreddit is that, every joke reaching the front page is a repost. 

Example:

<blockquote class="reddit-card" data-card-created="1578141860"><a href="https://www.reddit.com/r/Jokes/comments/5vt0bn/girls_call_me_ugly_until_they_find_out_how_much/">Girls call me ugly until they find out how much money i make.</a> from <a href="http://www.reddit.com/r/Jokes">r/Jokes</a></blockquote>
<script async src="//embed.redditmedia.com/widgets/platform.js" charset="UTF-8"></script>

<blockquote class="reddit-card" data-card-created="1578141883"><a href="https://www.reddit.com/r/Jokes/comments/aysxx6/girls_used_to_call_me_ugly_until_they_found_out/">Girls used to call me ugly until they found out how much money I make.</a> from <a href="http://www.reddit.com/r/Jokes">r/Jokes</a></blockquote>
<script async src="//embed.redditmedia.com/widgets/platform.js" charset="UTF-8"></script>

<blockquote class="reddit-card" data-card-created="1578141900"><a href="https://www.reddit.com/r/Jokes/comments/bugejh/women_call_me_ugly_until_they_find_out_how_much/">Women call me ugly until they find out how much money I make.</a> from <a href="http://www.reddit.com/r/Jokes">r/Jokes</a></blockquote>
<script async src="//embed.redditmedia.com/widgets/platform.js" charset="UTF-8"></script>

<blockquote class="reddit-card" data-card-created="1578141914"><a href="https://www.reddit.com/r/Jokes/comments/crclls/a_women_called_me_ugly_until_she_found_how_much/">A women called me ugly until she found how much money I make.</a> from <a href="http://www.reddit.com/r/Jokes">r/Jokes</a></blockquote>
<script async src="//embed.redditmedia.com/widgets/platform.js" charset="UTF-8"></script>

So, as stated in objective, I wanted to quantitatively find out exactly what fraction of submitted jokes are repost.  

# Finding reposted jokes

## Basic Approach

I started with very basic approach to find if the given joke is already submitted. If the given joke matched exactly, character for character, with already submitted joke then it's a repost. So, even if one of the character is here or there, the logic will not find it as duplicate. So according to this basic approach, all the four jokes in the example will be considered as different joke.

{% include figure image_path="/assets/images/r_jokes/repetition_basic.png" alt="basic" caption="Total of 3562 jokes were repeated, character for character, at least once." %}

Even then, I found that there are 4.46% (3562) of the jokes are repeated character for character at least once. That's a staggering number. But the approach is definitely flawed as one can see from the example. So, I needed an approach which can take care of small variations.



## Advanced Approach

For more advanced approach, I used [Vector Space Model](https://en.wikipedia.org/wiki/Vector_space_model) which represents any text document as an algebraic vector. So, every joke is now can be represented as an vector. Now, the similarity between two jokes can be computed as one can compute similarity between two vector i.e. by using cosine dot product. 
I used [scikit-learn's TF-IDF vectorizer](https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.TfidfVectorizer.html) to vectorize the jokes and [cosine similarity matrix](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.pairwise.cosine_similarity.html) to find out the similarity. In depth discussion about these functions can be found on [Christian S. Perone's blogpost](http://blog.christianperone.com/2011/09/machine-learning-text-feature-extraction-tf-idf-part-i/). 

# Onto the Results

{% include figure image_path="/assets/images/r_jokes/repetition_advanced.png" alt="advanced" caption="Total of 3562jokes were repeated at least once." %}

As you can see from the above graph, only YY% of total jokes are original. ZZ% of the jokes were posted more than once. These are very large numbers. 

## Most submitted joke

## Median upvotes as a function of number of times the joke was submitted

## Temporal Distribution of reposting

