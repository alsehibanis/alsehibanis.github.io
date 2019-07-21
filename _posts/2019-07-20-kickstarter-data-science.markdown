---
layout: post
title:  "Data Science Blog: Analyzing Kickstarter Projects"
date:   2019-07-20 18:26:29 +0300
categories: Datascience 
author: "Sultan Alsehibani"
---
![Cover](/assets/cover.png)
### Introduction
Individuals are beginning to play a new important role in various markets, by providing capital and investment support to projects and ideas. This method of supporting and investing in new projects is called crowdfunding. Crowdfunding is a group effort of people who pool their money together in order to invest and support projects initiated by other people. Moreover, many successful service businesses that organize crowdfunding have emerged. One of the most famous service businesses that organize crowdfunding is Kickstarter.

### Kickstarter
In Kickstarter, a person or organization could create a project and set a fund goal for that project. After that, investor's, called backers, would invest and support that project for a certain award. Kickstarter has three main guideline that project creators need to follow. The guidelines are meant to reinforce that individuals are backing projects and not ordering products. Projects can be categorized into one of 13 categories and 36 subcategories.

### Why Kickstarter Projects?
As it is the case with new projects, certain projects become successful and others fail. For people who would like to start new projects on Kickstarter there are uncertainties. In this blog, we would like to show how we can use insights from previous projects to help new starters to focus their efforts in the most suitable direction. 

### Blog Questions

{:refdef: style="text-align: center;"}
![Questions](/assets/q.jpg)
{: refdef}

This blog aims to answer three questions:
1. Q1: How accurately can we predict the success of a project on Kickstarter using historical data?
2. Q2: What are the top five categories that were most funded on Kickstarter?
3. Q3: What are the top five subcategories that were most funded on Kickstarter?

The dataset that I will be using contains of over 300,000 Kickstarter projects. This dataset is provided by **Mickaël Mouillé** on Kaggle [dataset](https://www.kaggle.com/kemical/kickstarter-projects)

### Methodology
In order to answer the three questions above the CRSIP-DM methodology was followed. CRISP-DM stands for cross-industry process for data mining and provides a structured approach to plan a data mining approach.

### First Question
In order to answer the first question, **How accurately can we predict the success of a project on Kickstarter**, three different machine-learning (ML) algorithms were evaluated to check how accurately we could predict the success or failure of a Kickstarter project. The three models are, Decision Tree, Random Forest, and Ada Boost.
The prediction models use the length of the campaign, the main category of the project, the subcategory, and the country of the project creator, and the currency of the goal set for the project to predict the status of the project.


After evaluating the three models, the best model (Ada Boost) was picked for fine-tuning. After fine-tuning the model, we found that we could predict the success or failure of a Kickstarter project with 68% accuracy.
{:refdef: style="text-align: center;"}
![Comparsion of different ML models](/assets/fig1.png)
{: refdef}

### Second Question
Our second quesiotn is **What are the top five categories that were most funded on Kickstarter?**. In order to answer that question we have analyzed the historical data of the Kickstarter projects. We found out that the top five categories that were most funded are Games, Design, Technology, Film & Video, and Music.
The figure below shows the amount of the total fund each main category received.

{:refdef: style="text-align: center;"}
![Top five categories](/assets/fig2.png)
{: refdef}

### Third Question
Our third question **What are the top five subcategories that were most funded on Kickstarter?**, which was answered through analyzing the historical data of the Kickstarter projects. We found out that the top five subcategories that were most funded are Product Design, Tabletop Games, Video Games, Hardware, and Documentary.
The figure below shows the amount of the total fund each main category received.

{:refdef: style="text-align: center;"}
![Top five subcategories](/assets/fig3.png)
{: refdef}

### Conclusion
In this blog we have answered three questions related to Kickstarter projects. The first question, was **How accurately can we predict the success of a project on Kickstarter** we have found that we can predict the success or failure of a project with 68% accuracy. Second, the answer to the second question **What are the top five categories that were most funded on Kickstarter?**, is Games, Design, Technology, Film & Video, and Music. Finally, the answer to the third question **What are the top five subcategories that were most funded on Kickstarter?**, is Product Design, Tabletop Games, Video Games, Hardware, and Documentary.

### Acknowledgment
I would like to thank **Mickaël Mouillé** for providing the dataset on Kaggle [dataset](https://www.kaggle.com/kemical/kickstarter-projects)