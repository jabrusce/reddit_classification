# Project 3: Web APIs & Natural Language Processing (NLP)

### Problem Statement
1. Use an API to collect data from two subreddits
2. Build a model using NLP to classify posts to predict if they are from within one of two subreddits.
4. Use this model to potentially aid in choosing which subreddit to post in or browse science and technology information in.

### Executive Summary
I chose the two subreddits, *r/science* and *r/technology*. I chose these two subreddits to compare one of the largest subreddits to a similarly focused subreddit to see if I could find any patterns in speech. r/science currently has approximately 26M members and is ranked #7 in subreddits by subscribers, while r/technology currently has approximately 10M members.

Using the Pushshift API, I was able to collect 5000 posts and 5000 comments from each subreddit starting January 16, 2021.

I used both the post and the comment data to explore trends in language, but **only used the posts data sets for modeling** (i.e. subreddit post titles) .

I fit this data on various NLP models including a Logistic Regression, Naive Bayes, Decision Trees, and Boosting models. I used various NLP techniques incuding Tfidf, Lemmatizer, and stop words to increase performance of the model.

When evaluating the models, I took into consideration the Balanced Accuracy Scores and the models number of false positives and false negatives. The best model I came accross was a Gradient Boost model which attained a balanced accuracy score of 0.88, f1 score of 0.89, and an ROC AUC score of 0.95.


### File Directory / Table of Contents
Data was collected using the Pushshift API to collect posts and comments from r/science and r/technology.

##### Python notebook files:
 - 01 Data Collection for posts and comments for both subreddits:
     - 01a_Data_Collection_posts.ipynb
     - 01b_Data_Collection_comments.ipynb
 - 02 Data Cleaning
     - 02a_Data_Cleaning_posts.ipynb
     - 02b_Data_Cleaning_comments.ipynb
 - 03 EDA
     - 03a_EDA_posts.ipynb
     - 03b_EDA_comments.ipynb
 - 04 Modeling
     - 04a_Modeling_Logistic_Regression.ipynb
     - 04b_Modeling_Naive_Bayes.ipynb
     - 04c_Modeling_Decision_Trees.ipynb
     - 04d_Modeling_Boosting.ipynb

##### Files contained in the 'data' folder:
   - 'comments_combined_clean.csv' - Combined comment data from comments in r/science and r/technology
   - 'posts_combined_clean.csv'- Combined post data from comments in r/science and r/technology
   - 'reddit_science_clean.csv' - r/science post data after cleaning
   - 'reddit_technology_clean.csv'- r/technology post data after cleaning
   - 'science_comments_clean.csv' - r/science comment data after cleaning
   - 'technology_comments_clean.csv' - r/technology comment data after cleaning
 
### Data Dictionaries
posts_combined_clean Data Dictionary
<br>

|Feature|Type|Dataset|Description|
|---|---|---|---|
|**subreddit**|*object*|posts|Subreddit|
|**title**|*object*|posts|Post Title| 
|**author**|*object*|posts|Post Author| 
|**created_utc**|*int*|posts|Time & date of post| 
|**upvote_ratio**|*float*|posts|Ratio of upvotes to downvotes| 
|**full_link**|*object*|posts|URL link of post| 
|**num_comments**|*int*|posts|Number of comments on post at time of pull|
|**num_crossposts**|*int*|posts|Number of crossposts on post at time of pull|
|**total_awards_received**|*int*|posts|Number of awards received on post at time of pull|
|**score**|*int*|posts|Score of post at time of pull|

### Data Cleaning

The data collected with the Pushshift API created mostly-clean dataframes. There were a few columns with mostly null-values that were dropped. Duplicate posts, including a large amount of "test posts" and comments were also dropped for EDA and modeling.

### EDA

##### Posts

EDA for the dataset included building a count vectorizer to analyze the words in the post names as well as the comments in both subreddits. The most frequent words in both subreddits included many stopwords. After removing stopwords, the most frequent words in r/science included many words related to Covid-19, human health, and science & research. The most frequent words in r/technology included words related to large tech companies and some words describing politics. 

Similarly, after analyzing the n-grams of the combined dataset, words related to Covid-19 and science, research, and climate change were noticed with bi-grams. For tri-grams, words such as 'Covid-19 pandemic', 'Covid-19 vaccine', and research were among the most common.

##### Comments

There was less of a clear distinction in the words used in r/science comments most common words, which included: people, life, covid, etc. The r/technology comments most common words included similar words but also included more political terms. The most frequent bigrams and trigams in the comments included 'life expectancy', 'minimum wage', 'social media', as well as web site addresses.


### Modeling
##### Base Model
The target in the dataset for posts was split approximately 57% for r/science. The base model will be 0.57 (57%) - if we guessed r/science each time we would be correct 57% of the time

##### Logistic Regression
Logistic Regression model passed through a pipeline and gridsearch and performed best with stopwords included, a c-value of 0.01, and 10,000 max iterations. Adding a Tfidf Vectorizer did not seem to make a large impact on the model. The model was slightly overfit, had a balanced accuracy score of 0.97 and an f-1 score of 0.89 with an accuracy on th etesting set of 0.87. The model had 330 false positives and 294 false negatives.

##### Naive Bayes Model
This model performed slightly worse than the the Logistic regression model. It had a lower accuracy score on the test set and the balanced accuracy and f-1 score was slightly lower. Most words that have a high weight in the model are large tech companies as well as some words that relate to politics. When the Tfidf vectorizer was added, the model did slightly worse than the MultinomialNB model. 

##### Decision Tree Model
The baseline decision tree is overfit - The model fit the training data almost perfectly, but fit the testing data worse. The f1 score and balanced accuracy scored worse than the Logistic and Naive Bayes models. The words which had the highest weight on the model included words relating to scientists/research/studies, as well as some tech company names.

##### AdaBoost Model
This model scored well without any parameter tuning with a balanced accuracy score of 0.85 and an f-1 score of 0.86.

##### Gradient Boost Model
This model performed the best out of all the models tested. It had an accuracy on the testing dataset of 0.887, a balanced accuracy score of 0.886, and an f1 score of 0.898. It also had the least amount of false positives and false negatives. The ROC AUC score was 0.95, which indicates the ratio between the true positievs and the false positive rates fit the model well. The features with the highest weights in the model include words related to Covid-19, studies/scientits, and some tech-company names.

### Conclusions and Recomendations
- The gradient boost model provides a fairly accurate classification and can be used to classify post titles from r/science and r/technology with an approximate 89% accuracy. This model performed better than the other models tested, including a Logistic Regression Model, Naive Bayes Model, Decision Tree Model, and an AdaBoost model.
- There is slightly more emphasis on politics in r/technology comments than in r/science.
- This model may aid someone looking to post in or browse these specific subreddits.
- To improve the model, we can:
    - Collect more data
    - Look at other tokenizers
    - Analyze n-grams further

### Sources
- r/technology: https://www.reddit.com/r/technology/
- r/science: https://www.reddit.com/r/science/
- Pushshift API: https://github.com/pushshift/api