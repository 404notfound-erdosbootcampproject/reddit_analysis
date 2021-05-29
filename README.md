# Analysis of Financial Subreddit Data

## Authors:
- Jason Liang
- Yixin Xu
- Berkan Yilmaz
- Cindy Zhang
- Jingjie Zhang
- Bradley Zykoski

## Project Overview
Online financial forums have started to have non-trivial impacts on market activity. Recent examples include:
- Elon Musk's tweets about cryptocurrencies and their influence on cryptocurrency prices
- The GameStop short squeeze, which was driven by posts on the subreddit r/wallstreetbets 

For a subreddit, it is therefore useful to have predictors for the "popularity" of a post. Our project defines a "popularity" metric and models this using features of a post that can be gathered within an hour of its posting.

## Description of the Data

We used the Python Reddit API Wrapper (PRAW) to scrape data on posts and their comments from the subreddit r/wallstreetbets. From each comment we gather the id of the parent post, which allows us to link the comment data to the post data.

We completed our initial analysis on a dataset of posts from several months on r/wallstreetbets. We observed that the popularity of the subreddit as a whole waxes and wanes over time, and so in our final analysis, we focus in on posts from a single week.

Anatomy of a Reddit post:
A post on Reddit has several key features. It has a title and a body, the latter of which may include non-text content. Users may assign upvotes and downvotes to a post, indicating their preference for the post. The difference between the number of upvotes and downvotes is the post's score. Users may add comments in two ways: they may comment directly on the post ("top-level" comments), or they may add a comment in response to another comment.

Given a particular post on r/wallstreetbets, we considered the following features:
- The length of the title
- The length of the body
- The score
- The ratio of upvotes to downvotes ("upvote ratio")
- The number of top-level comments on the post
- The number of top-level comments in the first hour
- The total number of all comments on the post

Our preliminary analysis showed that the data for meme posts and non-meme posts is shaped very differently, so for this project, we decided to restrict our analysis to non-meme posts.

![#of comments vs score](https://user-images.githubusercontent.com/81650558/120056329-7845a180-c009-11eb-8793-5a2694a3b9c1.png)


## Measuring Popularity

It is of course nontrivial to decide precisely what one means by "popular." For our purposes, we say that a post is popular if:
- It is well-liked
- It generates much discussion

We therefore single out the post's score, upvote ratio, and total number of comments as relevent to measuring the post's popularity. From this three variables, we would like to distill a single numerical "popularity score" for a post. In order to do this, we perform a Principal Component Analysis on these three variables across our dataset. Our "popularity score" is then defined to be the principal score, that is to say, the first principal component from our analysis. This principal score explains 45% of the variance in our normalized scores, upvote ratios, and comment numbers.

## Features that Influence Popularity

Our goal is to "catch on early" to the fact that a post is becoming popular. On the outset of our project, we decided to collect the following features.

The most direct thing one might expect to do is to see whether the post has quickly generated a lot of discussion. We therefore gather data on how many (top-level) comments there are on a post within an initial time period. There is of course a tradeoff here: the shorter the time period, the less nascent discussion we get to observe, but the longer the time period, the longer we have to wait to act on our results!

The coarsest observation one can make about a post is simply how long it is. It is quite sensible to expect <i>a priori</i> that this should affect the resulting popularity of the post. Short posts might become more popular: after all, there is a reason "too long; didn't read" (TL;DR) has become a common acronym! On the other hand, long posts might also become more popular: after all, there's more content to discuss! Perhaps there is some truth to both of these considerations: we did not find title length or body length to be useful variables.

Finally, we felt it would be reasonable to assess the overall sentiment of the words being posted. We therefore used VADER Sentiment Analysis to produce analytics on various sentiment variables in the titles and bodies of posts. Unfortunately, we found very little use for these variables.

Since the only meaningful feature that we found to influence popularity is the number of early comments, we intend to use this feature as best we can. We gather the number of comments in the first 30, 60, 90, and 120 minutes of the post's lifetime. We treat each of these values as its own variable; using these variables together with each other acts as a measure of frequency. We tested our model with Thus our model uses 4 input variables.

The correlation of the principal score with various variables was :
title_length           0.001149
time_created           0.079461
first_30_comments      0.470774
first_hour_comments    0.527607
first_90_comments      0.540926
num_comments           0.552987
first_120_comments     0.553477
class                  0.590673
score                  0.601900
upvote_ratio           0.628804
num_top_comments       0.745683
principal              1.000000

## Model

If a post's principal score is greater than the median, we call the post "popular." Otherwise, we call it "unpopular." Therefore we aimed to have a model that correctly classified posts significantly more than 50% of the time. 

We first attempted to classify the popular posts using the KNN algorithm. After testing the algorithm for up to 20 neighbors, we found that the average accuracy over 5 test-training splits never exceeded 20%. Since this accuracy was too low, we decided to abandon this method.

Next, we attempted to create a decision tree for our model using the DecisionTreeClassifier subpackage from the sklearn package. We used the nubmer of comments from the first 30 and first 60 minutes as inputs. Our tree correctly classified posts 60% of the time. This is a non-trivial improvement, since 49.4% of the posts are popular.

![tree](https://user-images.githubusercontent.com/81804685/120056209-fd27bf80-bff7-11eb-9b99-005d78b5a5f9.png)

One drawback of the decision tree method is sensitive to changing the inputs. To test this without data, we performed 100 splits with n=5 and plotted the accuracy of each split. The average accuracy was 59.95%, with a median accuracy of 62.2%. Therefore most splits produce a model that is meaningfully more accurate than just random guessing.

![60min](https://user-images.githubusercontent.com/81804685/120056426-56dcb980-bff9-11eb-8672-37ae056b0115.png)

If we also add the number of comments from the first 90 minutes and the first 120 minutes as inputs, the average accuracy increases to 62.8% and the median accuracy to 64.4%.

![120min](https://user-images.githubusercontent.com/81804685/120056439-6e1ba700-bff9-11eb-8ba9-913fa75e8a65.png)

## Conclusion

We created a decision tree that uses the number of comments made in the first half-hour and within the first-hour a post is made on r/wallstreetbets to predict whether it will become popular, i.e. reach the top 50% in the "top" posts of the day. Our tree correctly classified the test data 60% of the time, which we view as an acceptable improvement over the baseline of 50% given that (i) our data was highly noisy and (ii) any such classification should be expected to be difficult (otherwise the subreddit could be easily gamed).


Going forward, we expect that the decision tree generated may be highly sensitive to the recent conditions of the subreddit, i.e. during the GameStop saga, the subreddit attracted many users from other parts of the site, which may have changed the demographics and voting characteristics. It may still be plausible to generate a decision tree every day, and to use this tree to classify posts from the next day. We thus hope to repeat this process over a long period of time and track our results.

Thank you for reading!
