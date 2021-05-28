# Analysis of Financial Subreddit Data

## Authors:
- Jason Liang
- Berkan Yilmaz
- Yixin Xu
- Cindy Zhang
- Jinjie Zhang
- Bradley Zykoski

## Project Overview
Online financial forums have started to have non-trivial impacts on market activity. Recent examples include:
- Elon Musk's tweets about cryptocurrencies and their influence on cryptocurrency prices
- The GameStop short squeeze, which was driven by posts on the subreddit r/wallstreetbets 

For a subreddit, it is therefore useful to have predictors for the "popularity" of a post. Our project defines a "popularity" metric and models this using features of a post that can be gathered within an hour of its posting.

## Description of the Data

We used the Python Reddit API Wrapper (PRAW) to scrape data on posts and their comments from the subreddit r/wallstreetbets. From each comment we gather the id of the parent post, which allows us to link the comment data to the post data.

(If using old data:
We collected data on posts from several months.)

(If using new data:
We completed our initial analysis on a dataset of posts from several months on r/wallstreetbets. 
We observed that the popularity of the subreddit as a whole waxes and wanes over time, and so in our final analysis, we focus in on posts from a single day.) 

Anatomy of a Reddit post:
A post on Reddit has several key features. It has a title and a body, the latter of which may include non-text content. Users may assign upvotes and downvotes to a post, indicating their preference for the post. The different between the number of upvotes and downvotes is the post's score. Users may add comments in two ways: they may comment directly on the post ("top-level" comments), or they may add a comment in response to another comment.

Given a particular post on r/wallstreetbets, we considered the following features:
- The length of the title
- The length of the body
- The score
- The ratio of upvotes to downvotes ("upvote ratio")
- The number of top-level comments on the post
- The number of top-level comments in the first hour
- The total number of all comments on the post

Our preliminary analysis showed that the data for meme posts and non-meme posts is shaped very differently, so for this project, we decided to restrict our analysis to non-meme posts. We similarly discard any posts with a particular short title or body, so that we focus our analysis on a single type of post: wordy non-meme posts.

## Measuring Popularity

It is of course nontrivial to decide precisely what one means by "popular." For our purposes, we say that a post is popular if:
- It is well-liked
- It generates much discussion

We therefore single out the posts score, upvote ratio, and total number of comments as relevent to measuring a post's popularity. From this three variables, we would like to distill a single numerical "popularity score" for a post. In order to do this, we perform a Principal Component Analysis on these three variables across our dataset. Our "popularity score" is then defined to be the principal score, that is to say, the first principal component from our analysis.

## Features that Influence Popularity



## Resources we used
- Resource 1: 
	- Used for blah blah blah



-----------------------------------------------------------
## Other

Whatever
