Executive summary of Project 3

Finding it hard to find volunteer moderators for your forums to enforce rules? Worried about these volunteer moderators becoming inactive and them holding onto admin access? Worried about too much clutter/spam building up into your forums that people would view your forum with lower regard?

Fret not
We propose the research,building and implementation of a NLP classifier model. The model here however would not be a multi-class classification model but rather a binary classification model. The goal of the project is to be a proof of concept that machine-learning classifier systems could work for forums. If the proof of concept model could work, management can reconsider allocating more budget to build a more enhanced deployment model for multiclass classification.

Based on our research, building and testing of models in the notebook below, we were able to utilise a countvectorizer and logistic regression to predict from which subreddit (r/nosleep or r/UnsolvedMysteries) a post is from with up to a accuracy of 99.39%. We would need more resources and time to research and test the feasibility to deploy it on a whole forum basis.

Our research process as usual..

The Data Science Process

Problem Statement
Data Collection
Data Cleaning & Preprocessing
EDA & Modeling
Evaluation and Conceptual Understanding
Conclusion and Recommendations

# 1. Problem Statement
---

To use NLP to train a classifier to predict from which subreddit (either r/nosleep or r/UnresolvedMysteries) a given post came from. This is a binary classification problem.


r/nosleep: Nosleep is a subreddit for realistic horror stories.
<br>r/UnresolvedMysteries: UnresolvedMysteries is a subreddit dedicated to the unresolved mysteries of the world.</br>

# 2. Data Collection
---

Data Collection is done in another notebook under Data Gathering. Codes are not in the main workbook to prevent long waiting time to loop through posts, download and override existing datasets and affect the results.

Brief steps of the process is discussed here.

<ol>
    <li>Using the python `requests` library to connect to the subreddit url via reddit's json</li>
    <li>Make sure that the status code is correct and properly connected.</li>
    <li>Wrote a loop and ran the loop parsing the base url and the query to go to the next 40 posts until it loops to collect 1000post and exports it to a csv file.</li>
    <li>Check the length of the list and the number of empty posts.</li>
    <li>Repeat for the other subreddit</li>
</ol>
<br>    
</br> 

# 3. Data Cleaning
---

Data (posts webscrapped from reddit) from the other notebook is imported here for further data cleaning.

Technically, there were no empty posts when we tried to gather data but when we import and check for null values in this workbook there are a total of 10 empty posts. The empty posts were post filled with a (space) in the post. The (space) character is not captured in the export/import of csv.

After dropping the rows containing the null stories, we proceed to clean the data by writing a function that:
<ol>
    <li>Remove HTML tags</li>
    <li>Remove non-letter, remove some html tags that people use on reddit to get extra "breaks" in the post, removing the sub reddit tags</li>
    <li>Convert to lower case, split into individual words</li>
    <li>In Python, searching a set is much faster than searching a list, so convert the stop words to a set.</li>
    <li>Remove stop words</li>
    <li>Lemmatize the words</li>
    <li>Join the words back into one string separated by space and return the result.</li>
</ol>

**we use the scikit learn stop words instead of the nltk's stop words as there are more stop works in it**

# 4. Preprocessing
---

**Here, we will proceed to shuffle and split the data into 3 parts: 1 part to be used for training corpus, 1 part as a first test data to see how well the model has performed to evaluate the model and another set of test data to the model to test.**     

# 5. EDA
---

We will be doing EDA here at this stage on the documents of r/nosleep and r/unsolvedmysteries to see if there are any distinct and overlapping words, the length of post etc.

We will be plotting some wordcloud, bar chart and historgrams.

# 6. Baseline model to predict
Each post has 40% to be an 'Unresolved Mysteries' post.

# 7. Models

## Pipeline creation

Our pipeline will consist of four parts:

**Naive Bayes**

**A. CountVectorizer method**
1. An instance of `CountVectorizer`
2. A Naive Bayes `MultinomialNB` instance

**B.TF-IDF Vectorizer method**
1. An instance of `TfidfVectorizer`
2. A Naive Bayes `MultinomialNB` instance

<br>

</br>

**Logistic Regression**

**C. CountVectorizer method**
1. An instance of `CountVectorizer`
2. A `LogisticRegression` instance

**D.TF-IDF Vectorizer method**
1. An instance of `TfidfVectorizer`
2. A `LogisticRegression` instance

# 8. Model evaluation

In this case, incorrectly mistaking a post for a r/UnresolvedMysteries post doesn't seem much better or worse than incorrectly mistaking it for a r/Nosleep post.
Because I believe false positives and false negatives are equally as bad, I'd probably use accuracy (which is also the model score).

Since **C.) the CountVectorizer & Logistic Regression and D. )the TF-IDF & Logistic Regression** combination gives the highest score. I decide that i will proceed with **C.) the CountVectorizer & Logistic Regression** as the params for TD-IDF seems counterintuitive to have a min df of 10.

# Conclusion

After testing the model with unseen data (blind_X), the model still works well with 99.4% accuracy.

Given that the top words for r/unsolvedmysteries have website url keywords inside like http, www & com. We might want to re-evaluate the model without such words as they can be a give away to classify the post.

We proceed below to create another model that drops the http, www, com words and it still works very well. 

