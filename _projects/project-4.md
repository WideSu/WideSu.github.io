---
title: "Project 4-Predict the bike rentals with Azure ML Designer"
excerpt: "MLops on Azure<br/><img src='/images/proj4.png'>"
collection: projects
---

In project, we need to come up with a model that can be used to predict bike rentals based on factors such as weather, temperature, day of the week, and more. We’ll use Azure Machine Learning studio and Azure Machine Learning designer to create a regression model that will predict bike rentals.

# Objective
<img width="723" alt="image" src="https://user-images.githubusercontent.com/44923423/182752597-24327cb4-5927-4056-84e5-0d0ec28d373a.png">
- Open the Azure ML Studio and Create a New Dataset
In the Azure ML studio, create a new dataset using the bike rental data file from Microsoft.

- Review the Dataset
Take a look at the data you’re working with and see if there are any missing values or values that will cause issues.

- Design a Pipeline to Predict Bike Rentals

Using the Azure Machine Learning designer, drag and drop modules to create a pipeline that will train a regression model to predict bike rentals. When setting the compute type, set it to compute cluster.

- Review the Results

Take a look at some of the metrics for the run. For example, the R2 score or the root mean squared error.

- Bonus: Optimization of the Model

As a fun bonus objective, feel free to tweak some settings in your pipeline to see if you can get better scores for your model.

# Create a New Dataset

Firstly, let's create a dataset on Azure Data Lake using web url of our dataset.

<img width="1349" alt="image" src="https://user-images.githubusercontent.com/44923423/182754429-e947c30c-0469-4270-8918-f1481a73554d.png">

<img width="973" alt="image" src="https://user-images.githubusercontent.com/44923423/182755236-8d7657a4-8bf7-4928-a416-5e73cc210278.png">

# Design a Pipeline

<img width="924" alt="image" src="https://user-images.githubusercontent.com/44923423/182753865-53c52890-fd69-488f-87ae-3c0d5b6f3469.png">

# Review the Results

<img width="442" alt="image" src="https://user-images.githubusercontent.com/44923423/182755041-fd389212-46ef-435a-a58a-00ca1563259e.png">

# Optimize the model

We can further optimise the model by using boosted decision tree.

<img width="1349" alt="image" src="https://user-images.githubusercontent.com/44923423/182754784-911253cb-5a10-4f4d-93f4-3c838f762e95.png">