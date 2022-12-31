# Predicting Reddit Post Engagement
This repository contains the code for the AML group project for MSBA Fall 22. 
Our goal is to predict whether a post will end up being a top or a controversial post on Reddit and give the user few recommendations on the same.

All the code can be found within the [Predicting_reddit_post_engagement.ipynb](https://github.com/praths007/predicting_reddit_post_engagement) notebook

The [Medium blog](https://medium.com/@coolskates111/predicting-a-reddit-posts-engagement-9e6be15c617a) on the same can help you to understand the project in more details.

This ReadMe is a brief overview of the project.


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

## Modeling
We will try to predict the engagement of a post using 3 different modeling approaches and select the best approach
### 1. Logistic Regression
We use logistic regression because it is easier to understand and has high explainability.

We use Regularization with L1 penalty(Lasso Regulatization) because  we have too many features associated with a post scraped from Reddit. Some of these features might not have any variance or importance towards determining a post being top or controversial. This type of regularization helps us to perform automatic feature selection. 

More info on L1 Regularization can be found [here](https://www.youtube.com/watch?v=NGf0voTMlcs).

For our evaluation, we will use metrics such as precision, recall, f score, support and AUC. 

Following are the results of Logistic Regression with L1 penalty:

![Lasso Results](/misc/lasso_results.png)

### 2. Random Forest
To capture non-linear pattern, we use Random Forest Classifier. 
Following are the results of Random Forest Classifier on default parameters:
![Random Forest Results](/misc/rf_results.png)

Here we see that the model AUC improves marginally but there is a significant improvement in the f1-score. It also reinforces the fact that the data has non-linear patterns and using a linear-model is insufficient. 
### 3. XGBoost with Bayesian Optimization 

We saw that we were getting better results in Random Forest hence next logical step to use boosting. We will be using XGBoost with Bayesian Optimization for hyperparameter tuning. 
We prefer to use Bayesian Optimization over GridSearch because we have a lot of parameters. As the number of parameters increases, the combinations for GridSearch also increase.
Bayesian optimization solves this by trying to model prior of the parameters in question

Following are the results of XGBoost with Bayesian Optimization:
![XGBoost Results](/misc/xgb_results.png)

## Feature Importance

In order to provide recommendations for higher engagement, we also give the most important features of a post that the user should focus on.
Our tree-based approaches can give us the feature importance, but a disadvantage with the same is that the importance from tree-based method tend to be biased towards high cardinality features and features that have continuous values take higher precedence

To overcome this, we use [SHAP feature importance](https://christophm.github.io/interpretable-ml-book/shap.html), an agnostic of the modeling technique. 

Following are the top 20 features according to the same:
![SHAP Results](/misc/shap_top20.png)

## Text Recommendations

In order to understand what to post(or what to include in a post) to generate high engagement, we use [KeyBERT](https://towardsdatascience.com/keyword-extraction-with-bert-724efca412ea)  to extract keywords and key phrases that are most relevant to the document 
Following are the bigrams that KeyBERT recommends we include in the title of the post for higher engagement:
```
[('viral friends', 0.3225),
 ('net neutrality', 0.3252),
 ('rescue cat', 0.3264),
 ('unbanning trump', 0.3267),
 ('cyberattacks sway', 0.3382),
 ('quarantine reminder', 0.3393),
 ('turned internet', 0.3414),
 ('neutrality save', 0.3441),
 ('snapchat dad', 0.3456),
 ('blog vitality', 0.3503)]
```

These are the bigrams we should stay away from as they appear more frequently in controversial posts:
```
[('agree criminals', 0.3392),
 ('integrity protestors', 0.3429),
 ('justice 2016', 0.3468),
 ('leftist anti', 0.347),
 ('conspiracies don', 0.3515),
 ('meeting anti', 0.3573),
 ('moscow murders', 0.3828),
 ('investigation dumbest', 0.4129),
 ('criminology podcast', 0.4153),
 ('supporters antifa', 0.4361)]
```
## References:

1. Predicting reddit engagement without using sentiment analysis (no use of NLP) - http://cs229.stanford.edu/proj2012/ZamoshchinSegall-PredictingRedditPostPopularity.pdf
2. http://cs229.stanford.edu/proj2011/PoonWuZhang-RedditRecommendationSystem.pdf
3. https://cloud.google.com/blog/products/gcp/predicting-community-engagement-on-reddit-using-tensorflow-gdelt-and-cloud-dataflow-part-2
4. Reddit scraping using PRAW -
https://www.geeksforgeeks.org/scraping-reddit-using-python/




