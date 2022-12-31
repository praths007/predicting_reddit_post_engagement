# Predicting Reddit Post Engagement
This repository contains the code for the AML group project for MSBA Fall 22. 
All of the code can be found within the [Predicting_reddit_post_engagement.ipynb](https://github.com/praths007/predicting_reddit_post_engagement) notebook

We will scrape data from Reddit for our analysis using PRAW. 



## Scraping from Reddit
[PRAW](https://praw.readthedocs.io/en/stable/) (Python Reddit API Wrapper) is an API which allows scraping of the website. More details for PRAW setup and API reference can be found [here](https://praw.readthedocs.io/en/stable/).

We scrape “[r/all](https://www.reddit.com/r/all/)” subreddit and extract the top and controversial posts. One can differentiate between the 2 scraping processes by looking at the method from the subreddit (subReddit.top or subReddit.controversial)

## Data Cleaning

For this project’s scope, we will only include the features that can be controlled by the user . Eg, while posting a user can control the title length, body length, subreddit name, edit time, etc. All features outside the user’s control — upvote ratio, number of comments, etc. are excluded from the model building process.

## Preprocessing and Feature Engineering
Moving towards feature engineering of our data, our goals on a high level are as follows:

1. Label encoding  
2. Extract information from datetime features
3. One hot encoding for nominal columns (months and session)
4. Treating null values 
5. Standard scaling of features 

## Heatmap
![Heatmap image](/misc/heatmap.png)
