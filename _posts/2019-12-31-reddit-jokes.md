---
title: Reposts on reddit.com/r/jokes
header:
  overlay_image: assets/images/snoopng.png
toc: true
author: Abhilash
excerpt: 
---

# Objective

To quantitatively find out what fraction of jokes submitted on [reddit.com/r/jokes](https://www.reddit.com/r/Jokes/) are reposts.

# Introduction

**Reddit** is an American social news aggregation, web content rating, and discussion website where registered members submit content to the site such as links, text posts, and images, which are then voted up or down by other members. Posts are organized by subject into user-created boards called "subreddits", which cover a variety of topics including news, science, movies, video games, music, books, fitness, food, and image-sharing.

One of the popular subreddit is [r/jokes](https://www.reddit.com/r/Jokes/) with 17.6 million "humorists"  subscribed. It is currently 19th largest subreddit with about 1500 jokes submitted per month (greater than 50 upvotes). 

```
{% include figure image_path="/assets/images/r_jokes/repost.png" alt="this is a placeholder image" caption="This is a figure caption." %}
```

# Reposting Problem

Though r/jokes is very large subreddit, it lacks the originality in submitted jokes. One of the running gags on the subreddit is that, every joke reaching the front page is a repost. So, as stated in objective, I wanted to quantitatively find out exactly what fraction of submitted jokes are repost.  

# Finding reposted jokes

## Basic Approach

I started with very basic approach to find if the given joke is already submitted. If the given joke matched exactly, character for character, with already submitted joke then it's a repost. So, even if one of the character is here or there, the logic will not find it as duplicate. Even with this flawed logic, I found that there are XX% of the jokes are reposted character for character. That's a staggering number. 

figure

## Advanced Approach

For more advanced approach, I used [Vector Space Model](https://en.wikipedia.org/wiki/Vector_space_model) which represents any text document as an algebraic vector. So, every joke is now can be represented as an vector. Now, the similarity between two jokes can be computed as one can compute similarity between two vector i.e. by using cosine dot product. 
I used [scikit-learn's TF-IDF vectorizer](https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.TfidfVectorizer.html) to vectorize the jokes and [cosine similarity matrix](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.pairwise.cosine_similarity.html) to find out the similarity. In depth discussion about these functions can be found on [Christian S. Perone's blogpost](http://blog.christianperone.com/2011/09/machine-learning-text-feature-extraction-tf-idf-part-i/). 

# Onto the Results

figure

As you can see from the above graph, only YY% of total jokes are original. ZZ% of the jokes were posted more than once. These are very large numbers. 

## Most submitted joke

## Median upvotes as a function of number of times the joke was submitted

## Temporal Distribution of reposting

