---
layout: post
title:  "Data Science Blog: Starbucks Challenge"
date:   2019-07-30 18:26:29 +0300
categories: Datascience
author: "Sultan Alsehibani"
---
{:refdef: style="text-align: center;"}
![Cover](/assets/starbucks/cover.jpg)
{: refdef}

### Introduction
Individuals are beginning to play a new important role in various markets, by providing capital and investment support to projects and ideas. This method of supporting and investing in new projects is called crowdfunding. Crowdfunding is a group effort of people who pool their money together in order to invest and support projects initiated by other people. Moreover, many successful service businesses that organize crowdfunding have emerged. One of the most famous service businesses that organize crowdfunding is Kickstarter.


### Project Overview

Offers are one of the ways businesses use to attract customers. However, people respond differently to different offers. This project aims to analyze the experiment data provided by Starbucks to decide what are the offers that excite people.

The data set contains simulated data that mimics customer behavior on the Starbucks rewards mobile app. Once every few days, Starbucks sends out an offer to users of the mobile app. An offer can be merely an advertisement for a drink or an actual offer such as a discount or BOGO (buy one get one free). Some users might not receive any offer during certain weeks.

Not all users receive the same offer, and that is a challenge to solve with this data set.

This data set is a simplified version of the real Starbucks app because the underlying simulator only has one product whereas Starbucks actually sells dozens of products.

**Note:** The notebook for this project can be found (here)[https://github.com/alsehibanis/Starbucks_Challenge]


### Problem Statement

This project aims to build a supervised learning model that predicts whether a customer will be influenced by a BOGO, or discount offers. The methodology of this project is as follows:
1. Evaluate three supervised learning models to predict whether a customer will be influenced by an offer or not. The evaluation will be based on the accuracy and f-score results. The three models are:
    a. Decision Trees
    b. Forests
    c. Ada Boost
2. Evaluate whether a subset of the data can be used to train the above models. As this might be useful when the data size grows in the future.
3. Fine-tune the best of the above models to find better parameters that will improve the accuracy and f-score results.
4. Highlight and discuss the most important features that are used by the chosen model

### Metrics
The evaluation of the different supervised learners will be based on the accuracy and f-score. Accuracy by it self is not a good metric to evaluate models except when the values of false positive and false negatives are almost same. Therefore, we will consider the f-score since the f-score is a weighted average of Precision and Recall. [(reference)](https://blog.exsilio.com/all/accuracy-precision-recall-f1-score-interpretation-of-performance-measures/)

### Data Sets

The data is contained in three files:

* portfolio.json - containing offer ids and meta data about each offer (duration, type, etc.)
* profile.json - demographic data for each customer
* transcript.json - records for transactions, offers received, offers viewed, and offers completed

## Data Exploration

#### Explaining the Portfolio Data
{:refdef: style="text-align: center;"}
![portfolio](/assets/starbucks/port.png)
{: refdef}

From the above we can see that the portfolio contains the following columns:
* id (string) - offer id
* offer_type (string) - type of offer ie BOGO, discount, informational
* difficulty (int) - minimum required spend to complete an offer
* reward (int) - reward given for completing an offer
* duration (int) - time for offer to be open, in days
* channels (list of strings)

It is good that there are no missing data in the portfolio

However, Since our objective (problem statement) is focused on BOGO, and discount offers. We will extract and drop all informational offer data.

#### Cleaning the Portfolio Data
We need to clean the portfolio as follows:
1. Rename `id` to `offer_id`
2. Extract all informational offers.
3. Create dummy variables for `offer_type` using one-hot-encode
4. Normalize `difficulty`
5. Create dummy variables for `channels`


### Explaining the Profile Data
{:refdef: style="text-align: center;"}
![Profile](/assets/starbucks/profile.png)
{: refdef}

* age (int) - age of the customer
* became_member_on (int) - date when customer created an app account
* gender (str) - gender of the customer (note some entries contain 'O' for other rather than M or F)
* id (str) - customer id
* income (float) - customer's income

#### Visualizing the Profile Data

From the age bar chart below we can see that there is an outlier at age 118. This is due to assigning the age 118 to any person who does not want to declare their age.
{:refdef: style="text-align: center;"}
![Profile](/assets/starbucks/age.png)
{: refdef}

From the income chart below we can see that their are no outliers and that the income chart does not follow a normal distribution.
{:refdef: style="text-align: center;"}
![Profile](/assets/starbucks/income.png)
{: refdef}


**Note** in the profile data there are 2175 rows with missing data. Since these rows are missing both income and age information, these rows will be dropped.

#### Cleaning the Profile Data
We need to clean the profile data as follows:
1. Drop all rows with missing values.
2. Encode the `age` based on decade. For example, 24 = 20's, 31 =30's and so on.
3. Extract the year from the `became_member_on` column.
4. Encode the `gender` with dummy variables.
5. Encode the `income` based on range where each step = 10,000.   

#### Explaining the Transcript Data
{:refdef: style="text-align: center;"}
![Transcript](/assets/starbucks/tran.png)
{: refdef}

* event (str) - record description (ie transaction, offer received, offer viewed, etc.)
* person (str) - customer id
* time (int) - time in hours since start of test. The data begins at time t=0
* value - (dict of strings) - either an offer id or transaction amount depending on the record

It is good that there are **no missing values** in our transcript data.

#### Extracting Offers From the Transcript Data
We need to extract the offers from the transcript data as follows:
1. Split the `value` column into two columns `offer_id` which holds the offer id, and `reward` which will hold the reward value if the offer type is offer completed.
2. Drop all informational offers

### Shuffle and Split Data
Now all categorical variables have been converted into numerical features, and all numerical features have been normalized. As always, we will now split the data (both features and their labels) into training and test sets. 80% of the data will be used for training and 20% for testing.

## Modeling and Evaluating Model Performance
In this section, we will investigate two different supervised learners and evaluate their performance in predict the success or failure of a project. Moreover, we will evaluate the model's performance using 1% of the data, 10% of the data, 100% of the data.

### Three supervised learners
1. Decision Trees
 + Strengths: Minimal data preparation, and suitable for both categorical and numerical data!
 + Weaknesses: Over-complex trees do not generalize the data well, and learning an optimal decision tree is known to be NP-complete!
 + Why this Model: Decision Trees tend to perform well at binary classification.
 + [`Reference1`](https://scikit-learn.org/stable/modules/tree.html)
 + [`Reference2`](https%3A%2F%2Fbooksite.elsevier.com%2F9780124438804%2Fleondes_expert_vol1_ch3.pdf&usg=AOvVaw2I_dUwcITKT-i_yi8iChLC)



2. Random Forest (Ensemble Method)
 + Strengths: Random Forests and Ensemble Methods generalize better that single estimators (e.g. Decision Trees)
 + Weaknesses: Random Forests might overfit if the data is noisy.
 + Why this Model: Random Forests is expected to overcome the overfitting limitation of decision trees (our first model)
 + [`Reference1`](https://scikit-learn.org/stable/modules/ensemble.html#forests-of-randomized-trees)
 + [`Reference2`](http://blog.citizennet.com/blog/2012/11/10/random-forests-ensembles-and-performance-metrics)
 + [`Reference3`](https%3A%2F%2Fwww.ncbi.nlm.nih.gov%2Fpmc%2Farticles%2FPMC4062420%2F&usg=AOvVaw1VQtVvD6EmyvewEpSR1TMI)



3. Ada Boost

   + Strengths: Adaboost is more robust than single estimators and provides improved generalizability.
 + Weaknesses: Depends on the classifier as it could lead to biased boosted model.
 + Why this Model: Adaboost is the one of most popular boosting algorithms.
 + [`Reference1`](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.AdaBoostClassifier.html)


### Evaluation
{:refdef: style="text-align: center;"}
![Evaluattion](/assets/starbucks/comp.png)
{: refdef}
From the visualizations above it looks like Adaboost has higher accuracy (69%) and f-score(68%) results on testing sets. However, Adaboost did not do well on the training set which indicates under fitting. Therefore, I choose the Random Forest classifier for further tuning. Random Forest has an accuracy of (64%) and an f-score of (63%) on the testing set

In addition, from the visualization it seems that using a subset of the data leads to very high accuracy and f-score during training which indicates overfitting. Therefore, I chose to use the full training set when optimizing the Random Forest classifier.

#### Fine-Tuning the Random Forest Classifier
By using grid search on two parameters, we have fine-tuned the random forest classifier. Below is the accuracy of the optimized model versus the unoptimized model versus naive predictor
{:refdef: style="text-align: center;"}
![Accuracy Comparison](/assets/starbucks/acc.png)
{: refdef}

In addition, the best parameters for the random forest model are shown below:
{:refdef: style="text-align: center;"}
![Best Parameters](/assets/starbucks/best.png)
{: refdef}


### Top Five features
Through the use of the feature importance property which measures the contribution of each feature to the model's prediction. We have found the top five features answered
1. income: customer's income
2. social: offer was sent through social channels
3. year_2018: the customer became member in 2018
4. year_2016: the customer became member in 2016
5. diffculty: minimum required spend to complete an offer.

Below is a chart the shows the contribution of each feature:
{:refdef: style="text-align: center;"}
![Top Features](/assets/starbucks/top.png)
{: refdef}

## Conclusion

In this project I have built a model that predicts wither a customer will be influenced by an offer or not. This project followed four main steps. First, cleaning the data and performing the necessary preprocessing transformations. Second, by understanding the data we have created a methodology to assess wither or not a customer was influenced by an offer. Next, we have compared the performance of three supervised learners to the naive predictor. The three supervised learners are decision trees, random forest, and Adaboost. Finally,  Random forest was chosen and its hyper-parameters were refined using grid search.


 Furthermore, by using the "Feature importance" property of the random forest classifier we have measured the feature's contribution in that model. Our final random forest classifier suggest that the most important five features are

 1. customer's income
 2. offer was sent through social channels
 3. The customer created an app account in 2018
 4. The customer created an app account in 2016
 5. The minimum required spend to complete an offer.

### Future Work
In the future additional supervised learning algorithms could be evaluated and included in the analysis.
### Acknowledgment
I would like to thank **Starbucks** for providing the dataset for this project through **Udacity**.
