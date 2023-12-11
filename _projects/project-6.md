---
title: "Project 6-Multilingual Named Entity Recognition"
excerpt: "NLP application<br/><img src='/images/proj5.png'>"
collection: projects
---

# Table of Contents

- Task Description
- Literature review: models that can do NER
- Result Evaluation
- Sample results for English Sentences
- Sample results for Chinese Sentences
- Limitations
- Challenges & Future Work

# Task Description
Named-entity recognition is a subtask of information extraction that seeks to locate and classify named entities mentioned in unstructured text into pre-defined categories such as person names, organizations, locations, medical codes, time expressions, quantities, monetary values, percentages, etc.

<p align="center">
<img width="500" alt="image" src="https://github.com/WideSu/Vanilla-NER/assets/44923423/9e751192-6aeb-4000-8bfc-9c40906030b9">
</p>

# Literature review: models that can do NER
<p align="center">
<img width="500" alt="image" src="https://github.com/WideSu/Vanilla-NER/assets/44923423/23d548d3-7b2d-43bd-a8fe-39ad27333612">
</p>
# Dataset
- Chinese corpos: flair.datasets.NER_CHINESE_WEIBO()
- English corpos: CoNLL++
<p align="center">
  <img width="500" alt="image" src="https://github.com/WideSu/Vanilla-NER/assets/44923423/58cb473b-a528-417a-8a79-eb1c71d9e59d">
</p>
# Data prep
Since our dataset is relatively clean already, we only removed stopwrods and did tokenization using the BERT tokenizer.
<p align="center">
<img width="500" alt="image" src="https://github.com/WideSu/Vanilla-NER/assets/44923423/3fdb19cc-9d59-4131-bb0f-c838dc78e603">
</p>
The ground truth data was in the shown format, in English , each word token is assigned a NE tag, in chinese, each character is assigned a NER tag.
<p align="center">
<img width="500" alt="image" src="https://github.com/WideSu/Vanilla-NER/assets/44923423/c5483d6d-ecba-455a-8bd8-e73ee1b24285">
</p>
# Our Model: BiLSTM-CRF
We combined Bi-directionary LSTM with conditional random field, which has better result than only using BiLSTM.

For deep learning based NER task, it usally has three steps:
1) Get the representation for the input. We use the pretrained word embedding BERT and Character-level embedding to 

In sequence tagging task, we have access to both past and future input features for a given time, we can thus utilize a bidirectional LSTM network to efficiently make use of past features (via forward states) and future features (via backward states) for a specific time frame. For the output layer, There are two different ways to make use of neighbor tag information in predicting current tags. The first is to predict a distribution of tags for each time step and then use beam-like decoding to find optimal tag sequences. The second one is to focus on sentence level instead of individual positions, thus leading to Conditional Random Fields (CRF) models. 
<p align="center">
<img width="500" alt="image" src="https://github.com/WideSu/Vanilla-NER/assets/44923423/e0c46299-220a-4e57-b962-9b1733066c64">
</p>

# Result Evaluation
GloVe is an unsupervised learning algorithm for obtaining vector representations for words. Training is performed on aggregated global word-word co-occurrence statistics from a corpus, and the resulting representations showcase interesting linear substructures of the word vector space.


Our model beats the Baseline "glove" for precision, recall, F1-score in recognizing organization, location, person,  and misc which means all names which are not already in the other categories except the recall for organization.
<p align="center">
<img width="500" alt="image" src="https://github.com/WideSu/Vanilla-NER/assets/44923423/c4bdfe1c-aca9-4e5f-8c8f-de512fbba5aa">
</p>

# Sample results for English Sentences
Here's some result for recognizing name entities in English sentences.

We choose some confusing sentences such as George Washington went to Washington to study in University of Washington .

<p align="center">
<img width="500" alt="image" src="https://github.com/WideSu/Vanilla-NER/assets/44923423/988520d3-f7aa-496b-81fe-3c2fd16e72e8">
</p>

# Sample results for Chinese Sentences
And here are some results for Chinese Sentences

<p align="center">
<img width="500" alt="image" src="https://github.com/WideSu/Vanilla-NER/assets/44923423/f89acb95-26ac-4de0-a4f4-c7fecdb145b8">
</p>

# Limitations

Since the Conll dataset we used is 2003, which is pretty outdated, we want to try inferencing with our model on new named entity which may be unseen in our training dataset. This is our result: (explain result: 1. although johnny depp was already famous at 2003, amber heard was still a nobody at that time , but our model can recognize her name. 2.similarly trump was also not so famous then, twitter even didn’t exist. 3. the boys is a pretty new TV series first aired in 2019 4. finally a model trained with 2003 dataset can correctly pick up covid-19). in general, our model perform quite well in recognizing new NE.

<p align="center">
<img width="500" alt="image" src="https://github.com/WideSu/Vanilla-NER/assets/44923423/aaa73792-28f3-4ded-b864-4be458c81d7e">
</p>

# Challenges & Future Work
For example, we tested model inference for Chinese corpus using Telsla, the model cannot detect it. 
Hence the future work includes:

<p align="center">
<img width="500" alt="image" src="https://github.com/WideSu/Vanilla-NER/assets/44923423/cbb48e08-1e21-4e4b-9e2c-156b3b32f5a6">
</p>
