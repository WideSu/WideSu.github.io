---
title: "Project 5-Which Tech Company will be the next Champion?"
excerpt: "NLP application<br/><img src='/images/proj5.png'>"
collection: projects
---

# Table of Contents

- 1. Problem Statement
- 2. Datasets
- 3. Data Cleaning
- 4. Select relevant data
- 5. Topic modeling
- 6. Sentiment analysis
- 7. Conclusion

## 1. Problem Statement

Our goal is to look at comments of various tech companies and note their similarities and differences. Specifically, I'd like to know if the Five tech giants are different than other companies, since they shared most of the market.

<img width="431" alt="image" src="https://user-images.githubusercontent.com/44923423/183739893-67f244fa-6d74-45e7-9419-009200ad113a.png">

<img width="592" alt="image" src="https://user-images.githubusercontent.com/44923423/183739675-15b71b17-2d79-4600-bf2f-6d99e9d72f8c.png">

## 2. Dataset

- 1.Employees’ comments : glassdoor.com
- 2.Public comments – tweets

## 3. Data Cleaning

Firstly, let's look at our data:

<img width="476" alt="image" src="https://user-images.githubusercontent.com/44923423/183740563-54c24445-7510-4a3e-8673-ea65b4a72723.png">

### 3.1 Numerical Data
When dealing with numerical data, data cleaning often involves removing null values and duplicate data, dealing with outliers, etc. With text data, there are some common data cleaning techniques, which are also known as text pre-processing techniques.

### 3.2 Textual Data
With text data, this cleaning process can go on forever. There's always an exception to every cleaning step. 
So, we have to follow a specific rule, in this project, we chose the **MVP** (minimum viable product) approach - start simple and iterate.

Here are a bunch of things you can do to clean your data. There're some common cleaning steps, on the left hand-side and the rest can be done at a later point to improve our results.

<img width="829" alt="image" src="https://user-images.githubusercontent.com/44923423/183741102-730743f3-a5df-4f48-b344-42a7a3d2ec89.png">

In our application, we decide to do the following steps:

- 1. Eliminate handles and URLs

- 2. Tokenize the string into words. 

- 3. Remove stop words like "and, is, a, on, etc."

- 4. Stemming- or convert every word to its stem. Like dancer, dancing, danced, becomes 'danc'. You can use porter stemmer to take care of this. 

- 5. Convert all your words to lower case. 

On that basis, we can draw a word cloud to show some common words. As can be seen, there are some meaningless words such as “got”. So, we should go back to the stop words removal part to fix the problem. Besides using the common stop words dataset, we created a list to add the stop words we want, literately, customize the stop words dataset. 

<img width="520" alt="image" src="https://user-images.githubusercontent.com/44923423/183742228-f4120657-4188-4613-a887-0b6bb4f35199.png">

## 4. Select relevant data
After data cleaning, we selected relevant tweets about tech companies using “sqldf”. Then, we visualized the numbers of tweets relating to each company. According to the bar graph, we find out our first piece of conclusion about the tech giants, **they are often talked about by people on social media.**

<img width="631" alt="image" src="https://user-images.githubusercontent.com/44923423/183742812-c108f6d5-2e5f-4e61-9dc9-3e0f5b696f52.png">

## 5. Topic modeling

Topic modeling is an unsupervised machine learning technique that’s capable of scanning a set of documents, detecting word and phrase patterns within them, and automatically **clustering word groups and similar expressions** that best characterize a set of documents.

<img width="1222" alt="image" src="https://user-images.githubusercontent.com/44923423/183743059-2d388ec1-84d3-4f50-80bc-127f3eb6eb6a.png">

First we calculated the ti-idf of the data and then use LDA to split the data into five topics. 

![image](https://user-images.githubusercontent.com/44923423/183743213-417ef64d-0e66-43c3-81c2-40dad8675e7d.png)

Then, we use a LDA for clustering the comments into 5 topics. 

![image](https://user-images.githubusercontent.com/44923423/183743264-83449db9-98e3-4604-a4c4-5d8b23d8cff6.png)

Our result about tweets relating to google:

<img width="836" alt="image" src="https://user-images.githubusercontent.com/44923423/183743357-94b50b53-073b-4476-90fc-f1a71c04a8b0.png">

Not ideal, so I lowered the max_df to filter these frequent words like “googl”

<img width="715" alt="image" src="https://user-images.githubusercontent.com/44923423/183743595-7adfc528-d27c-4c53-838d-e9c2f5d927ba.png">

Here is the result we got after adjusting. However, it is still not that great. 

<img width="960" alt="image" src="https://user-images.githubusercontent.com/44923423/183743686-784109fa-7faf-4624-9825-75c1204d111b.png">

Though there’re no “googl” now,
The topic modeling result is still not very ideal……

### Result analysis

So I searched on the internet, I found that **LDA** performs **bad in short passages**. **Biterm Topic Model** performs **better*. So we will try that next time.

And we can do the topic modeling with domain expertise
We will try that in the future!!
Refered to: *Anchored CorEx: Topic Modeling with Minimal Domain Knowledge, 2017*

<img width="820" alt="image" src="https://user-images.githubusercontent.com/44923423/183744137-d01df62c-50f2-4d09-95c9-25cc2bbe382e.png">

The possible explanation is that the tweets are often short, repetitive, and full of abbreviation, so that words appear many times as key words in different groups. Searching on the internet, we found a better model which is biterm topic model to solve the problem. Besides, we can use anchored CorEx which considers the domain expertise when doing topic modeling. Here is its core codes. And it has a good result on the newspaper dataset. Like the war and Israel are divided into the same topic. So we may use it in the future project to test its performance. 

## 6. Sentiment analysis

"When a company gets bigger, it must bear some bad reviews on the Internet"

<img width="528" alt="image" src="https://user-images.githubusercontent.com/44923423/183744432-77faec95-d5e0-4812-830d-623972e7e8dc.png">

The second part is about sentiment analysis using R. First, we calculated the negative rates of tweets relating to tech company. Surprisingly, we find that when a company becoming larger, its negative comments often grow at the same time. 


We first classified the sentence into negative and positive sentence, and for each type of comment we drew a word cloud. 

<img width="634" alt="image" src="https://user-images.githubusercontent.com/44923423/183744557-f446531f-457b-4e27-956f-ca3858c1aaab.png">

For positive comments, the representative words are like ‘good profits’, ‘great culture’, ‘challenges’, and ‘life balance’. Those words picture a lucrative, humanitarian company. 
<img width="634" alt="image" src="https://user-images.githubusercontent.com/44923423/183744702-02fa0549-0019-4a51-9d0a-5092898c288d.png">

While for negative comments, the key words are full of smart people and hard to move up, which means the company is very competitive inside.

<img width="679" alt="image" src="https://user-images.githubusercontent.com/44923423/183744796-07c39f2e-cbac-44b9-8e2b-808b2d715fc2.png">

In addition, we plot the ratings for each company and we found that all of the ratings are higher than 3.5 and facebook is absolute the best of best. 

<img width="607" alt="image" src="https://user-images.githubusercontent.com/44923423/183744916-da8c3cd3-2bb8-44e7-8e32-f8a4a4a692a4.png">

## 7. Conclusion

To sum up, we summarized the common points of high tech companies which includes popular in social media, high reputation among employees and great challenges inside the company.

<img width="630" alt="image" src="https://user-images.githubusercontent.com/44923423/183745030-f28d8d88-d585-4b03-96e2-7b5294030005.png">

<img width="429" alt="image" src="https://user-images.githubusercontent.com/44923423/183745092-47a99375-27a2-4d13-94c7-21fc7b094f65.png">




