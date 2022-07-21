---
title: "Project 1-Predictive maintenance(time series)"
excerpt: "Predictive maintenance for diamond cutter machines<br/><img src='/images/proj1.png'>"
collection: projects
---

## Project results and final model

Achieved a test-set Recall rate of 0.958 on the final XGBoost model.

|FIELD1       |RR    |AC    |PS    |F1    |AUC   |
|-------------|------|------|------|------|------|
|XGBoost      **|0.9576|0.9583|0.9916|0.9743|0.9638|**
|Random Forest|0.8122|0.9179|0.7773|0.7944|0.9139|
|LightGBM     |0.6677|0.9337|0.9897|0.7974|0.9551|
|SVM          |0.5167|0.8358|0.5911|0.5514|0.6557|
|LSTM         |0.7342|0.9762|0.9765|0.8382|0.9449|

## Project Objective

Perform **EDA** and build **ML pipeline** that can predict whether a machine will break down after a period of time(binary classification) based on the parameters collected by sensors on production machines to give early warning and avoid unnecessary losses caused by downtime in the production process with recall rate.

The project is divided into 7 separate steps with 7 experiments in total:
- Step1: Perform EDA & data processing
- Step2: Identify evaluation metrics
- Step3: Conduct feature engineering for time-series data
- Step4: Build classification type ML pipeline
- Step5: Justify selection of models
- Step6: Deploy the model on MLflow

## Step1: Perform EDA & data processing
We takes three datasets as inputs.

Training dataset: It is the machines alarm data.
Validation dataset: It is the machines without failure events recorded.
Testing dataset: It is the machines without failure events recorded.
Ground truth data: It contains the information of true alarm alerts for each engine in the testing data.
The data schema for the training and testing data is shown in the following table.

|Column name|MySQL database|Dataframe|Description|
|-|-|-|-|
|flag|text|object|A slot of inspection|
|acttime|datetime|datetime64[ns]|time for this record, **unique**|
|param1|double|float|value for this parameter|
|...|double|float|value for this parameter|
|param86|double|float|value for this parameter|
|label|boolean|boolean|whether has alarm or not(0 means no alarm,1 means alarm)|

### Exploratory Data Analysis
The objective of this data analysis is to 
- understand the distribution of alarms in the time dimension
- find outliers
- check missing data

![image](https://user-images.githubusercontent.com/44923423/172553606-65bc7606-7ad7-4cb4-b219-3047ebdf0259.png)
![image](https://user-images.githubusercontent.com/44923423/172553783-4d3ed651-3139-4e4d-9222-8d8a7611c72b.png)
![actual_postion_r_traveler vanish](https://user-images.githubusercontent.com/44923423/172554920-e6053214-a493-4d07-9253-915ad647e580.png)

### Data processing
There are two situations for the data produced by the machine each time, one is that an alarm occurs during the period, and the other is that there is no alarm. If an alarm occurs in this production flag, the (t_alarm-t_pre-t_sampling, t-t_pre) data of each alarm data is aggregated with a time window t_w, and the statistical value (maximum value, minimum value, mean, standard deviation, slope, etc.), and label the corresponding alarm. If there is no alarm in the production flag this time, a time window will be used to aggregate the entire piece of data and the label of no alarm will be added.

The time window t_w and the sampling time t_sampling are adjustable. In this experiment, we tried the time window t_w of 1, 2, and 3 minutes, and checked the performance of the model on the test set, and found that t_w=2 minutes worked best.

Sampling time t_sampling tried 5min, 10min, 20min, 25min and found that a long sampling time (25min) works best.

![image](https://user-images.githubusercontent.com/44923423/164638155-ff98432c-dd01-4fc6-9a0b-cb2c0c1efc8c.png)

## Step2: Identify evaluation metrics

The evaluation metrics is recall rate

## Step3: Conduct feature engineering for time-series data

I tried different methods of feature selection: statistic filtering methods, embedded methods, and wapper methods. They have pros and cons 
<img width="917" alt="image" src="https://user-images.githubusercontent.com/44923423/179951005-e418cf62-e495-421b-b552-a1d869b1bc3d.png">
reference: Chandrashekar, Girish, and Ferat Sahin. "A survey on feature selection methods." Computers & Electrical Engineering 40.1 (2014): 16-28.

For statisitic filtering methods, such as

Selected the five most important features.
Union of 4 feature selection does not lead to significantly different results. 
Feature Intersection experiment is ongoing.  
5,10,30 min -> rolling window features
try different features
![image](https://user-images.githubusercontent.com/44923423/164643137-1964cf79-90a1-4721-8333-65a11e3aa6aa.png)

```
def get_features(data, label, agg, features):
    data.set_index("flag")
    if data.shape[0] >= rolling_window_size:
        data = data.groupby("flag")[features].rolling(window=rolling_window_size).agg(agg)
    elif data.shape[0] < rolling_window_size:
        data = data.groupby("flag")[features].agg(agg)
    # The aggregated dataframe has multi-index, which needs to be flattened into a single index
    data.columns = data.columns.to_flat_index()
    data = data.dropna().drop_duplicates().reset_index()
    data['label'] = label
    return data
```
## Step3: Build regressor ML pipeline

Changed 

## Step5: Justify selection of models
## Step6: Deploy the model on MLflow


## Q&A
### Why we do binary classification?

There are three tasks in predictive maintenance:

- Regression: Predict the Remaining Useful Life (RUL), or Time to Failure (TTF).
- Binary classification: Predict if an asset will fail within certain time frame (e.g. days).
- Multi-class classification: Predict if an asset will fail in different time windows: E.g., fails in window [1, w0] days; fails in the window [w0+1,w1] days; not fail within w1 days

Since the alarm data is highly skewed, we can only get good results on the most frequent dataset. (other alarms are too little and the precision is only 30% ) So we limited our scope to be binary classification which can give us recall rate more than 95%. 
