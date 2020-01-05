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

To demonstrate this method, take the example above. There are four similar jokes, and let us add one more "control" dissimilar joke to test the method. 

<blockquote class="reddit-card" data-card-created="1578205073"><a href="https://www.reddit.com/r/Jokes/comments/ivui2/woman_looking_in_the_mirror_broke_down_to_tears/">Woman looking in the mirror broke down to tears. Told her husband she looks fat, old, and ugly, said a compliment would make her feel better...</a> from <a href="http://www.reddit.com/r/Jokes">r/Jokes</a></blockquote>
<script async src="//embed.redditmedia.com/widgets/platform.js" charset="UTF-8"></script>
This is a good choice for control joke, as it has some words common with original joke. The similarity matrix between these jokes and one control joke is:

|        | Joke 1 | Joke 2 | Joke 3 | Joke 4 | Control Joke |
| :----: | :----: | :----: | :----: | :----: | :----------: |
| Joke 1 |   1    |  0.87  |  0.90  |  0.81  |     0.24     |
| Joke 2 |  0.87  |   1    |  0.73  |  0.70  |     0.17     |
| Joke 3 |  0.90  |  0.73  |   1    |  0.90  |     0.24     |
| Joke 4 |  0.81  |  0.70  |  0.90  |   1    |     0.22     |

As one can see from the table, the similarity score between first four jokes is at least 0.7. Whereas, the dissimilar control joke has similarity score of around 0.2. So, this approach seems to be able to handle slight variations within similar jokes.

# On to the Results

## Repetition of Jokes

{% include figure image_path="/assets/images/r_jokes/repetition_advanced.png" alt="advanced" caption="70.61% of the jokes were repeated at least once." %}

As you can see from the above graph, 70.61% of the jokes were posted more than once. There are only ~38k unique jokes out of analyzed ~79k jokes.

## Extended Graph

{% include figure image_path="/assets/images/r_jokes/repetition_advanced_ext.png" alt="advanced_ext" caption="Around 200 jokes have been posted 50 times." %} 

## Most submitted joke

The following joke was submitted 54 times!

<blockquote class="reddit-card" data-card-created="1578233219"><a href="https://www.reddit.com/r/Jokes/comments/4maoti/a_father_buys_a_lie_detector_robot_that_slaps/">A father buys a lie detector robot that slaps people when they lie.</a> from <a href="http://www.reddit.com/r/Jokes">r/Jokes</a></blockquote>
<script async src="//embed.redditmedia.com/widgets/platform.js" charset="UTF-8"></script>



## Temporal Distribution of reposting

# Limitations 

Short and template jokes are very difficult to differentiate. Knock knock jokes, how many X does it take to change a lightbulb, are typical examples. Although, these jokes are different essentially, the logic I used clubbed them together.

# Conclusion

Around 70% of the jokes submitted on reddit.com/r/jokes are posted more than once. There are some jokes which have been posted 50 times. Only ~38k out of  ~79k jokes are unique.