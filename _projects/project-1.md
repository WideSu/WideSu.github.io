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



