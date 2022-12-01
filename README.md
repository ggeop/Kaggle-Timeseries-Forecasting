![image](https://user-images.githubusercontent.com/35366798/205144588-41ccb4bc-e49d-4290-a526-1743ca5c1822.png)
# Introduction ⚡

In this notebook you will find a solution based on CatBoost. This algorithm is developed by Yandex and its famous the categorical feature handling (this is the reason for "cat" prefix in the name), easy parameter tuning and last but not least, the easy GPU scalability without extra tuning.

Based on the Yandex the algorithm term is the following:
*CatBoost is an algorithm for gradient boosting on decision trees. It is developed by Yandex researchers and engineers, and is used for search, recommendation systems, personal assistant, self-driving cars, weather prediction and many other tasks at Yandex and in other companies, including CERN, Cloudflare, Careem taxi. It is in open-source and can be used by anyone.*

*NOTE: In this notebook it's not included the EDA, it contains only what is related with CatBoost model*

## Things that worked 🔥
* One global per Family. That means 33 isolated models.
    * NOTE: Each Family model is a global model because it contains one timeseries for each store (54 timeseries)
* Set the categorical columns
* Create many features based on promotions (total, per family, rolling avgs, lags etc)
* The logarithm transformation (`log1p`) in target
* The Upgini extra features for `LIQUOR,WINE,BEER`, `CLEANING` product families for a slight improvement.
* Add `sample_weight` in the fitting with extra weight from `2017-07-01`

---

## Things that not gone well 💩
* RandomForest, ExtraTrees and other Gradient Boosting algorithms (XGBoost, LightGBM) had worse perfomance in my case.
* Model approches which didn't worked so well:
   * One global model
   * One global model per Store
* Feature transformations (MinMaxScaler, StandardScaler). The main models I worked were tree based models (RandomForest, ExtraTrees, XGBoost, LightGBM etc) which are models where feature scaling doesn’t make a difference.
* PCA for dimentionality reduction
* Feature Selection ([SelectKBest](https://scikit-learn.org/stable/modules/generated/sklearn.feature_selection.SelectKBest.html#sklearn.feature_selection.SelectKBest),[RFECV](https://scikit-learn.org/stable/modules/generated/sklearn.feature_selection.RFECV.html#sklearn.feature_selection.RFECV))
* Upgini features for all families
* A training sub-period. Instead of using the data from 2013, I tried to use a smaller range of dates e.g starting from 2014, 2015
..

---

## Conclusion 💁

**This solution has public score [0.4074 in Kaggle open competition](https://www.kaggle.com/code/mscgeorges/catboost-model-per-family). In the time of the publication was in the top 10% of the submissions!**

Yandex had made amazing job with the CatBoost implementation, its amazing how easily and FAST work out of the box. 
Before, CatBoost I achieved a score below 0.40 with ExtraTrees & BaggingRegressor, but the solution was much more complex and slower (it took approximately 5hours+)

The GPU acceleration is also a great feature!! I achieved to run a so powerful model in less than an hour 😍. Also Kaggle support free GPU acceleration with 30hours of quotas for a period of time, which is more than enough for this problem.

---

## Special Thanks 👏
Last but not least, I would like to thanks other competitors solutions for their great ideas:
* A great EDA: [Store Sales TS Forecasting - A Comprehensive Guide by Ekrem Bayar](https://www.kaggle.com/code/ekrembayar/store-sales-ts-forecasting-a-comprehensive-guide/notebook)
* A compact initial solution: [Simple TS + Ridge + RF](https://www.kaggle.com/code/dkomyagin/simple-ts-ridge-rf/notebook) by KDJ2020
* A nice idea of Linear Regression and Random Forest [Store_Sales_Using_LR_And_RF](https://www.kaggle.com/code/mr0024/store-sales-using-lr-and-rf)

---
