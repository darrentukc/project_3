<img src="http://imgur.com/1ZcRyrc.png" style="float: left; margin: 20px; height: 55px">

# Project 3 - Classifying Aliens and Space Subreddit Posts

## Background

Reddit is a very large and popular forum, with users across different countries and topics one can not even think of. There are many subreddits, which are communities for like minded people to post and share knowledge and information. Among these subreddits are 'aliens' and 'space'.

## Problem Statement

##### What we plan to do

Men In Black (MIB) is an entity that researches & monitors the sentiment of people on aliens, whether conspiracies that are made are a little too close to things that the people should not know about. Most of this 'information leak' will end up in either /aliens or /space. Instead of shutting down reddit or deleting the subreddits, moderating r/aliens could be an efficient way of monitoring.

Thus, they are requesting us to create a model to classify reddit posts and identify the key words that differentiates the post from "Aliens" to "Space". This can allow them to focus more of their efforts on r/aliens while using the model to flag out posts in r/space that potentially should be in r/aliens.

Also, the model should be able to generate a list of important words that not only are used to classify the posts, but to give a general sense of the topics of most of the posts. This information can also be useful for other associations to promote its marketing effort on social media, events and podcasts, that in turn can feed information to the mainstream.'

##### What models will we be exploring

Three classification models, Naive Bayes, Logistic Regression and Random Forest will be developed to assist with the problem statement, with logistic regression being the most whitebox among the 3. Along with these classifiers, Count Vectorizer and Tfidf Vectorizer will be used.

##### How will success be evaluated?

The success of the model will be assessed based on its F1 score on unseen test data. Accuracy and F1-score will be our main metrics to evaluate model performance and we would like to minimze it ideally.

##### Is the scope of the project appropriate? Who are our important stakeholders and why is this important to investigate?

This model will facilitate MIB's intelligence collection efforts on sentiments of people on extra terrestrial topics by predicting whether a post belongs to aliens or space. While doing so, important words that impact the decision making of the model can be listed out to be used by other departments to increase the efficiency of marketing on social media and any future events.

## Contents:

1. [Datasets Used](#1.-Datasets-Used)
2. [Data Dictionary](#2.-Data-Dictionary)
3. [Importing Data](#3.-Importing-Data)
4. [Combine Dataframes](#4.-Combine-dataframes)
5. [Feature Engineering](#5.-Feature-Engineering)
6. [EDA](#6.-EDA)
7. [Cleaning](#7.-Cleaning)
8. [Modeling](#8.-Modeling)
9. [Scoring and Evaluating Models](#9.-Scoring-Models)
10. [Choice of Production Model](#10.-Choice-of-Production-Model)
11. [Conclusion](#11.-Conclusion)


### 1. Datasets Used

The following datasets were used for this projects:
- alien.csv
- space.csv
These datasets were obtained by scrapping the subreddit Space and Aliens. There are 4500 Space posts and 2400 Aliens posts.


### 2. Data Dictionary

The below is a data dictionary containing all the data features, type and its description:


|Feature|Type|Description|
|---|---|---|
|**title**|*object*|Title of the post.| <br>
|**selftext**|*object*|Content of the post.| <br>
|**aliens**|*int64*|Subreddit post belongs to, 1 for 'aliens', 0 for 'space.| <br>
|**post_length**|*int64*|Length of post.| <br>
|**title_length**|*int64*|Length of title.| <br>
|**combined**|*object*|title + selftext.| <br>


### 3. Cleaning Train Dataset

Here, we clean up the dataset in order to do exploratory data analysis. We replaced all spaces in column names to underscore and all caps to lower case and converted year columns to age.

### 4. Combine Dataframes

In this section, we combined both datasets, and chose only title, selftext and the subreddit the post belongs to. The subreddit column is then renamed as aliens, our target variable.

### 5. Feature Engineering

Here, we added 2 columns, post_length and title_length. This is for further EDA below.


### 6. EDA

Here, we found that:

- There are many posts with post_length of 1, and many of them contain links to images and videos.
- The 25th percentile of post_length is 1, and 75% is 24, with a max of 7017. These outliers will be removed further down.
- Some of the posts contain non english characters, (arabic and chinese). These characters will be removed.

### 7. Cleaning

- Remove all null values from selftext
- Remove [removed] and [deleted]
- Lowecase posts and titles for further processing
- Remove links and emojis
- Remove markdown text formatting
- Remove Non English characters
- Remove outliers, post_length < 3, post_length > 414

### Further Processing and EDA

- Tokenize text, remove stopwords and lemmatize words
- Checked top unigrams and bigrams for each subreddit

### 8. Modeling

We trained 3 classifiers with 2 vectorizers, a total of 6 models. Out of the 6, we chose Tfidf Multinomial Naive Bayes, Count Vectorizer Multinomial Naive Bayes and Tfidf Logistic Regression to further tune hyperparameters.

### 9. Scoring

Of the 3 chosen models, we got a F1 score of between 88% to 90%, which is relatively good. A high F1 score represents that both Precision and Recall scores are high, which is what we were aiming for. The AUC Curve score for 3 models were also good, between 96% to 97%.

### 10. Choice of Production Model

Logistic Regression with TFIDF vectorizer

Of the 3 models, all of them scored well. All of them were able to accurately classify posts from aliens to space. But we chose logistic regression with Tfidf Vectorizer as it is the simplest model and the coefficients are easily interpretable, which results in us being able to extract the important features of the model for the secondary audience, the different marketing departments from MIB.

We managed to extract the following:

Alien Important Features:
|Feature|Coefficient|
|---|---|
|alien|6.33|
|ufo|3.36|
|specie|2.44|
|human|1.77|
|exraterrestrial|1.56|

Space Important Features:
|Feature|Coefficient|
|---|---|
|space|-4.69|
|telescope|-2.70|
|launch|-2.49|
|jwst|-2.05|
|moon|-1.66|

### 11. Conclusion

Recalling our problem statement:

>Men In Black (MIB) is an entity that researches & monitors the sentiment of people on aliens, whether conspiracies that are made are a little too close to things that the people should not know about. Most of this 'information leak' will end up in either /aliens or /space. Instead of shutting down reddit or deleting the subreddits, moderating r/aliens could be an efficient way of monitoring.
Thus, they are requesting us to create a model to classify reddit posts and identify the key words that differentiates the post from "Aliens" to "Space". This can allow them to focus more of their efforts on r/aliens while using the model to flag out posts in r/space that potentially should be in r/aliens.

>The success of the model will be assessed based on its F1 score on unseen test data. Accuracy and F1-score will be our main metrics to evaluate model performance and we would like to minimze it ideally. We should also be able to interpret feature importance from the model to identify key words the key words that seperates the 2 subreddits.

All the models trained were able to classify the posts accurately, with a F1 score between 89% to 91% and an AUC of 96% to 97% on unseen data. We chose Logistic Regression with TFIDF vectorizer as it not only gives a good F1 score, it is easily interpretable.

>Also, the model should be able to generate a list of important words that not only are used to classify the posts, but to give a general sense of the topics of most of the posts. This information can also be useful for other associations to promote its marketing effort on social media, events and podcasts, that in turn can feed information to the mainstream.'

The model makes its predictions by using the coefficients of each word in the post and calculates the odds of it belonging to alien subreddit or it not belonging to alien subreddit.

The top words with the highest 'weights' for alien subreddit ( in decreaseing order ) are:
- alien
- ufo
- specie(s)
- human
- extraterrestrial

The more the above listed words appear in a post, the higher the chance if it belonging to alien subreddit. We can also infer from these words that the topics on r/aliens are on aliens, ufos and extraterrestrials.

The top words with the highest 'weights' for space subreddit ( in decreaseing order ) are:
- space
- telescope
- launch
- moon
- jwst
 <br><br>

The more the above listed words appear in a post, the higher the chance if it belonging to alien subreddit. We can infer from these words that the topics on r/space are about space, James Webb Space Telescope and perhaps the launching of rockets to the moon.

With these important words, media could influence topics by using social media marketing, Search Engine Optimizations, events or even podcasts.

##### Limitations of Model

- We used the textual content of the post to train our model, as it is only able to process text, and left out videos, images and links to other websites.
- The data scrapped from reddit is from 1 Jan 2022. It is not an indicator of future or past. The results generated from the model only work for the period of 1 Jan 2022.
- There are posts that are more ambiguous in nature, which our model has a hard time predicting. Misclassification seems to occur when the post is made up of ambiguous words ( words with coefficients close to 0 ) and 1 or 2 words with strong correlation ( words with high absolute coefficients ).

##### Future Improvements

- Sentiment analysis could be done on the reults of the model, allowing MIB to have an idea of the general sesntiment of people and plan for their policies.
- Process information from text in image and transcript of videos could be added to the data to train a better model, as most of the data lost was through images and videos.
