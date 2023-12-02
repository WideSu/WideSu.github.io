---
title: "Project 1-Predictive maintenance(time series)"
excerpt: "Predictive maintenance for diamond cutter machines<br/><img src='/images/proj1.png'>"
collection: projects
---
## Tasks and schedule

<iframe width="100%" height="800" src="files/project1.pdf">

## Project results and final model

Since the company wants to **predict every alarm that may occur**, I use **recall rate** as the most important metric. Besides, I also records other accuracy metrics including accuracy, precision, f1 score and AUC score for selecting the best model.

We tried 5 models including:
- RF: Random forest, which is an ensemble learning method for classification, regression and other tasks that operates by constructing a multitude of decision trees at training time.
- [XGBoost](https://machinelearningmastery.com/gentle-introduction-xgboost-applied-machine-learning/): XGBoost is an implementation of gradient boosted decision trees designed for speed and performance.
- LightGBM: Light GBM is a fast, distributed, high-performance gradient boosting framework based on decision tree algorithm, used for ranking, classification and many other machine learning tasks.
- SVM: Support vector machines (SVMs) are a set of supervised learning methods used for classification, regression and outliers detection.
- LSTM: LSTM is a special kind of recurrent neural network that deals with long-term dependencies. It can remember information from past data and is capable of learning order dependence in sequence prediction problems.

Finally, we achieved a test-set Recall rate of 0.874 on the final XGBoost model. 

Why we choose XGBoost? It's true that random forest performs slightly better on recall(our main metric) but **XGBoost has a good performance on all metrics**. And it also **performs stable** when we **change the predict time, sample time**.

|FIELD1       |RR    |AC    |PS    |F1    |AUC   |
|-------------|------|------|------|------|------|
|XGBoost      **|0.874|0.9631|0.9916|0.9176|0.9667|**
|Random Forest|0.8931|0.8132|0.591|0.6923|0.9285|
|LightGBM     |0.8426|0.9728|0.9984|0.|0.9139|
|SVM          |0.5167|0.8358|0.5911|0.5514|0.6557|
|LSTM         |0.7342|0.9762|0.9765|0.8382|0.9449|

<img width="373" alt="image" src="https://user-images.githubusercontent.com/44923423/180921941-c462fb49-c705-4fb7-8d0f-13462fa0c9a9.png">


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

### Data processing
There are two situations for the data produced by the machine each time, one is that an alarm occurs during the period, and the other is that there is no alarm. If an alarm occurs in this production flag, the (t_alarm-t_pre-t_sampling, t-t_pre) data of each alarm data is aggregated with a time window t_w, and the statistical value (maximum value, minimum value, mean, standard deviation, slope, etc.), and label the corresponding alarm. If there is no alarm in the production flag this time, a time window will be used to aggregate the entire piece of data and the label of no alarm will be added.

The time window t_w and the sampling time t_sampling are adjustable. In this experiment, we tried the time window t_w of 1, 2, and 3 minutes, and checked the performance of the model on the test set, and found that t_w=2 minutes worked best.

Sampling time t_sampling tried 5min, 10min, 20min, 25min and found that a long sampling time (25min) works best.

![image](https://user-images.githubusercontent.com/44923423/164638155-ff98432c-dd01-4fc6-9a0b-cb2c0c1efc8c.png)

## Step2: Identify evaluation metrics

The evaluation metrics is recall rate since the company wants to detect every alarm that may occur. So recall rate is the prior metric that we are concerned about. Besides, we also need to make sure that the accuracy, precision of detecting alarms is not bad.

## Step3: Conduct feature engineering for time-series data

Feature selection methods are classiﬁed into three major categories: ﬁlter methods (are independent of learning models), wrapper methods (rely on learning models for selection criteria), and embedded methods (embed feature selection into learning models to achieve model fitting simultaneously).

<img width="392" alt="image" src="https://user-images.githubusercontent.com/44923423/180345204-2aae71ca-894e-458a-a863-9fb82a83ef95.png">


### Filter methods: ([SelectKBest](https://scikit-learn.org/stable/modules/generated/sklearn.feature_selection.SelectKBest.html) in Sklearn)
Filter methods measure the relevance of features by their correlation with dependent variable while wrapper methods measure the usefulness of a subset of feature by actually training a model on it.

Pros:
- computationally light
- Avoid overfitting
- Independent of any model


Cons: 
- Filter methods might **fail to find the best subset of features** in many occasions but wrapper methods can always provide the best subset of features.
- Ignore feature dependencies

#### Correlation Criteria
Pearson’s correlation: Capture only linear relationship between 2 targets
Spearman’s correlation: nonparamatic and capture monotonic relationship
chi-square: distribution relationship between 2 variables

#### Mutual Information
KL-divergence to depict dependencies between 2 variables
For discrete probability distributions {\displaystyle P}P and {\displaystyle Q}Q defined on the same probability space, {\displaystyle {\mathcal {X}}}{\mathcal {X}}, the relative entropy from {\displaystyle Q}Q to {\displaystyle P}P is defined[11] to be

{\displaystyle D_{\text{KL}}(P\parallel Q)=\sum _{x\in {\mathcal {X}}}P(x)\log \left({\frac {P(x)}{Q(x)}}\right).}


### Wrapper methods: ([SelectFromModel](https://scikit-learn.org/stable/modules/generated/sklearn.feature_selection.SelectFromModel.html) in Sklearn)
Wrapper methods measure the usefulness of a subset of feature by actually training a model on it. To get the best subset of features, there are two ways to do it, backward elimination and RFE (Recursive Feature Elimination)
1) Backward Elimination
 We feed all the possible features to the model at first. We check the performance of the model and then iteratively remove the worst performing features one by one till the overall performance of the model comes in acceptable range.
2) RFE (Recursive Feature Elimination)
The Recursive Feature Elimination (RFE) method works by recursively removing attributes and building a model on those attributes that remain. It uses accuracy metric to rank the feature according to their importance. The RFE method takes the model to be used and the number of required features as input. It then gives the ranking of all the variables, 1 being most important. It also gives its support, True being relevant feature and False being irrelevant feature.


Pro: wrapper methods can always provide the best subset of features.
Cons: 
- 1) computationally very expensive; 
- 2) using the subset of features from the **wrapper methods** make the model **more prone to overfitting** as compared to using subset of features from the filter methods.

### Embedded methods
Embedded methods combine the qualities’ of filter and wrapper methods. It’s implemented by algorithms that have their own built-in feature selection methods.
Some of the most popular examples of these methods are LASSO and RIDGE regression which have inbuilt penalization functions to reduce overfitting.

#### Lasso
objective is to minimize the loss while enforcing a constraint on the weights of the features.

Lasso is L1 regression.
From a practical standpoint, L1 tends to shrink coefficients to zero whereas L2 tends to shrink coefficients evenly. L1 is therefore useful for feature selection, as we can drop any variables associated with coefficients that go to zero. L2, on the other hand, is useful when you have collinear/codependent features.

```{python}
reg = LassoCV()
reg.fit(X, y)
print("Best alpha using built-in LassoCV: %f" % reg.alpha_)
print("Best score using built-in LassoCV: %f" %reg.score(X,y))
coef = pd.Series(reg.coef_, index = X.columns)
```


### Conclusion:
We saw how to select features using multiple methods for Numeric Data and compared their results. Now there arises a confusion of which method to choose in what situation. Following points will help you make this decision.

Filter method is less accurate. It is great while doing EDA, it can also be used for checking multi co-linearity in data.
Wrapper and Embedded methods give more accurate results but as they are computationally expensive, these method are suited when you have lesser features (~20). And in the future we can also use transformers for feature selection which can automatically specify the num of features.

In our experiement, we selected the five most important features by EDA and domain knowledge, and then use 2, 3, 5,10,30 min to do rolling window features, found out that 2 mins can give the best recall rate. So we keep this.
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
## Step4: Build classification type ML pipeline

The aim to build pipeline is to try different ways of feature engineering and see which way is better. For instance, we have the XGBoost algorithm, we can 
```{python}
pipe = Pipeline([
('scaler', StandardScaler()),
('selector', VarianceThreshold()),
('classifier', xgbc())
])
```
We can specify different scaler.
```{python}
parameters = {'scaler': [StandardScaler(), MinMaxScaler(),
	Normalizer(), MaxAbsScaler()],
	'selector__threshold': [0, 0.001, 0.01]
}
```
The pipe along with the above list of parameters are then passed to a BayesSearchCV() object, that searches the parameters space for the best set of parameters as shown below:
```{python}
grid = BayesSearchCV(pipe, parameters, cv=2).fit(X_train, y_train)
 
print('Training set score: ' + str(grid.score(X_train, y_train)))
print('Test set score: ' + str(grid.score(X_test, y_test)))
```
By tuning the pipeline, we can achieved quite an improvement over a simple classifier and a non-optimized pipeline.

Besides, we also tried different hyperparameter tunning packages.

|Library     |Pros                                                                    |Cons                                                           |Scenario                                                    |
|------------|------------------------------------------------------------------------|---------------------------------------------------------------|------------------------------------------------------------|
|Dask        |Run model with lazy loading;                                            |Only support models with partial fit interface;                |Memory constrained loading and training;                    |
|H2O         |Systematic and well wrapped;                                            |Only 2 basic methods (grid/random)                             |Fast development and deployment;                            |
|Scikit-learn|Flexible and basic;                                                     |Only 2 basic methods (grid/random); New methods are not stable;|Tradictional tuning                                         |
|HyperOpt    |High-speed and flexible; New search method: TPE/ATPE;                   |Out-of-date interface                                          |Time-limited                                                |
|Ray         |Systematic and well wrapped;                                            |Too customized and not flexible; Time-cost on initialization   |Fast development and deployment with various tuning methods;|
|Optuna      |Well-performed and light; Include all popular and stable tuning methods;|Not well wrapped for all methods                               |Accurate, flexible required                                 |



## Step5: Justify selection of models
After experimenting the models, we find that XGBoost can give the best result. Why? XGBoost is a decision-tree-based ensemble Machine Learning algorithm that uses a gradient boosting framework. XGBoost and gradient boosting machines are both ensemble tree methods that apply the principle of boosting weak learners using the gradient descent architecture.

### Model result interprebility

#### SHAP value
Using SHAP value from game theory to estimate how each feature contribute to the prediction results.

<img width="419" alt="image" src="https://user-images.githubusercontent.com/44923423/180346922-15a86da4-b9c8-40e9-a319-78cb0488f07d.png">


#### Visualise the change

![actual_postion_r_traveler vanish](https://user-images.githubusercontent.com/44923423/172554920-e6053214-a493-4d07-9253-915ad647e580.png)


## Step6: Deploy the model on MLflow
<img width="439" alt="image" src="https://user-images.githubusercontent.com/44923423/180350219-9ba6a410-ca76-4f63-b98f-265993a7bf69.png">



## Q&A
### Why we do binary classification?

There are three tasks in predictive maintenance:

- Regression: Predict the Remaining Useful Life (RUL), or Time to Failure (TTF).
- Binary classification: Predict if an asset will fail within certain time frame (e.g. days).
- Multi-class classification: Predict if an asset will fail in different time windows: E.g., fails in window [1, w0] days; fails in the window [w0+1,w1] days; not fail within w1 days

Since the alarm data is highly skewed, we can only get good results on the most frequent dataset. (other alarms are too little and the precision is only 30% ) So we limited our scope to be binary classification which can give us recall rate more than 95%. 
### How do you process data for LSTM model?
By changing data into batches with its lable. For instance, in our senior, every half hour has a label of alarm or non-alarm.
```{python}
def gen_sequence(id_df, seq_length, seq_cols):
    df_zeros=pd.DataFrame(np.zeros((seq_length-1,id_df.shape[1])),columns=id_df.columns)
    id_df=df_zeros.append(id_df,ignore_index=True)
    data_array = id_df[seq_cols].values
    num_elements = data_array.shape[0]
    lstm_array=[]
    for start, stop in zip(range(0, num_elements-seq_length), range(seq_length, num_elements)):
        lstm_array.append(data_array[start:stop, :])
    return np.array(lstm_array)

# function to generate labels
def gen_label(id_df, seq_length, seq_cols,label):
    df_zeros=pd.DataFrame(np.zeros((seq_length-1,id_df.shape[1])),columns=id_df.columns)
    id_df=df_zeros.append(id_df,ignore_index=True)
    data_array = id_df[seq_cols].values
    num_elements = data_array.shape[0]
    y_label=[]
    for start, stop in zip(range(0, num_elements-seq_length), range(seq_length, num_elements)):
        y_label.append(id_df[label][stop])
    return np.array(y_label)
```

### What's the bussiness value of this project?
The companies have saved a lot for maintaining those expensive diamond cutter machines.
<img width="731" alt="image" src="https://user-images.githubusercontent.com/44923423/180347170-ba32afa5-955b-4656-b911-c3abe84226db.png">