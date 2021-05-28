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
- The GameStop short squeeze, which was driven by posts on the subreddit WallStreetBets 

For a subreddit, it is therefore useful to have predictors for the "popularity" of a post. Our project defines a "popularity" metric and models this using features of a post that can be gathered within an hour of its posting.

## Description of the Data

We used the Python Reddit API Wrapper (PRAW) to scrape data on posts and their comments from the subreddit r/WallStreetBets. From each comment we gather the id of the parent post, which allows us to link the comment data to the post data.

(If using old data:
We collected data on posts from several months.)

(If using new data:
We completed our initial analysis on a dataset of posts from several months on r/WallStreetBets. 
We observed that the popularity of the subreddit as a whole waxes and wanes over time, and so in our final analysis, we focus in on posts from a single day.)

Our preliminary analysis showed that the data for meme posts and non-meme posts is shaped very differently, so for this project, we decided to restrict our analysis to non-meme posts. 

Anatomy of a Reddit post:
A post on Reddit has several key features. It has a title and a body, the latter of which may include non-text content. Users may assign upvotes and downvotes to a post, indicating their preference for the post. The different between the number of upvotes and downvotes is the post's score. Users may add comments in two ways: they may comment directly on the post ("top-level" comments), or they may add a comment in response to another comment.

Given a particular post on r/WallStreetBets, we considered the following features:
- The length of the title
- The length of the body
- The score
- The ratio of upvotes to downvotes
- The number of top-level comments on the post
- The number of top-level comments in the first hour
- The total number of all comments on the post

## Resources we used
- Resource 1: 
	- Used for blah blah blah



-----------------------------------------------------------
## Other

Whatever
