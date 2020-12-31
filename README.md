# COVID-19-CASES-PREDICTION
## Table of contents 
* [Background](#background)
* [Data Collection](#data-collection)
* [Data Preprocessing](#data-preprocessing)
* [Data Processing](#data-processing)
* [Modeling](#modeling)
* [Conclusion](#conclusion)

## Background
A novel coronavirus is a new coronavirus that has not been previously identified. The virus causing coronavirus disease 2019 (COVID-19), is not the same as the coronaviruses that commonly circulate among humans and cause mild illness, like the common cold [1](https://www.cdc.gov/coronavirus/2019-nCoV/index.html). 

California is reporting the fourth-highest rate of new COVID-19 cases per capita in the country, and the state's hospitals are overwhelmed and considering rationing care. Several regions have gone under new stay-at-home orders in recent weeks [2](https://www.sfgate.com/bayarea/article/California-COVID-lockdown-cases-deaths-businesses-15819841.php).

At this time, learning about California statistics and their relations with COVID-19 cases numbers is very meaningful. It gives us understanding about why some regions have more cases and others don't. It will also give us ideas about how to prevent COVID-19 spread more efficiently.  

## Data Collection

The datasets are selected and downloaded from [Census Bureau website](https://www.census.gov/quickfacts/fact/table/US/PST045219) and [Cailifornia Open Data Portal](https://data.ca.gov/dataset/covid-19-cases). One dataset is the statistics of all California couties in 2019 and the other one is the COVID-19 cases of all counties from 3/17/2020 to 12/17/2020. The following image shows the total confirmed case number for each county.

<img src="https://github.com/yingchuwang/COVID-19-CASES-PREDICTION/blob/main/case%20map.png" width="500">

## Data Preprocessing

Both datasets need to be preprocessed for further cleaning and modelling. The statistics data and cases data are preprocessed through the following steps.  

1. __Statistics data__ - [Data link](https://github.com/yingchuwang/COVID-19-CASES-PREDICTION/blob/main/CA_census.csv)
   * Rotate the table. Make rows the county names and columns the census data.
   * Delete the 'County, California' in the county name.
   * Clean the columns title so that they can be accepted by MySQL.
   * Deal with the cell format and change all format to 'number' format.
   * Delete the 'FIPS CODE' column.
   * Delete the cell value with 'D','F','C'.

2. __Cases data__ - [Data link](https://github.com/yingchuwang/COVID-19-CASES-PREDICTION/blob/main/CA_statewide_cases.csv)
   * Obtain the data only on 12/17/2020 using MySQL system.
   * Only keep the column 'totalcountconfirmed'.

3. __Concatenate and export__ - [Data link](https://github.com/yingchuwang/COVID-19-CASES-PREDICTION/blob/main/data.csv)

   * After preprocessing, the statistics data and cases data are concatenate and exported it as a csv file. This csv file will be used for further analysis.

## Understanding data

The Jupyter notenook is used here for analysis. The Jupyter Notebook is an interactive environment for running code in the browser. It is a great tool for exploratory data analysis and is widely used by data scientists. The Jupyter Notebook makes it easy to incorporate code, text, and images. The dataset is analized through the following steps:

1. __Import the dataset into Jupyter notebook__.

2. __Show the information of the dataset__. First 10 rows are exhibited as follow. 
   * The dataset has 58 rows and 63 columns.
   * The data type of columns 'County' is object. The other columns are numerical. 
   * 9 columns have missing values.

```
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 58 entries, 0 to 57
Data columns (total 63 columns):
 #   Column                                                       Non-Null Count  Dtype  
---  ------                                                       --------------  -----  
 0   County                                                       58 non-null     object 
 1   Population 2019                                              58 non-null     int64  
 2   Population 2010                                              58 non-null     int64  
 3   Population percent change                                    58 non-null     float64
 4   Population Census 2010                                       58 non-null     int64  
 5   Under 5 years                                                58 non-null     float64
 6   Under 18 years                                               58 non-null     float64
 7   65 years and over                                            58 non-null     float64
 8   Female persons                                               58 non-null     float64
 9   White alone                                                  58 non-null     float64
```

3. __Obtain the correlation matrix__ 

From correlation matrix, linear relationship (correlation) between any two numerical features is uncovered. Some variables have high correlation with others. These highly correlated features should be removed to avoid performance loss in model.
<img src="https://github.com/yingchuwang/COVID-19-CASES-PREDICTION/blob/main/correlation_matrix.png" width="600">

4. __Identify the correlation coefficient between features and target__. First 10 rows are shown as follow.

Features                                                    |totalcountconfirmed
------------------------------------------------------------|-------------------
totalcountconfirmed                                         |1.000000
Minority owned firms                                        |0.983799
Population 2010	                                          |0.981193
Population Census 2010                                      |0.981179
Women owned firms	                                          |0.979227
Population 2019                                             |0.978279
Total nonemployer establishments	                           |0.976220
Nonveteran owned firms                                      |0.976181
All firms	                                                |0.975531
Men owned firms                                             |0.974269

5. __Determin redundant features__

According to the correlation matrix and correlation coefficient, features have high correlation with others or have low correlation with the target will be removed. The following are the redundant features.

```
'Population 2010', 'Population Census 2010', 'Veterans', 'Housing units', 'Building permits', 
'Households', 'Total accommodations and food services sales', 
'Total health care and social assistance receipts or revenue', 'Total manufacturers shipments', 
'Total merchant wholesaler sales', 'Total retail sales', 'Total employer establishments', 
'Total employment', 'Total annual payroll', 'Total nonemployer establishments', 'All firms', 
'Men owned firms', 'Women owned firms', 'Minority owned firms', 'Nonminority owned firms', 
'Veteran owned firms', 'Nonveteran owned firms', 'Under 18 years', 
'Median household income in 2019 dollars', 'Median value of owner occupied housing units',
'Median selected monthly owner costs with a mortgage', 
'Median selected monthly owner costs without a mortgage', 'Households with a broadband internet subscription',
'Bachelor degree or higher', 'Per capita income in past 12 months in 2019 dollars', 
'Households with a broadband Internet subscription', 'Language other than English spoken at home', 
'In civilian labor force, female', 'Under 18 years', 'White alone, not Hispanic or Latino', 
'Persons per household'
```
   
## Data Processing

1. __Remove redundant features__ : After removing redundant features, the total number of columns has reduced to 27.

2. __Fill missing values__ : After removing the redundant features, there are two features have missing values and each one has only one missing value. The missing values are replaced with mean value of each feature.

Features                                                    |Missing value
------------------------------------------------------------|--------------
Total employment, percent change	                          |1
Native Hawaiian and Other Pacific Islander alone	          |1

## Modelling

1. __Split the dataset__

```
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.2, random_state = 0)
```

2. __Feature scaling__

```
from sklearn.preprocessing import StandardScaler
scaler = StandardScaler()
X_train_s = scaler.fit_transform(X_train)
X_test_s = scaler.transform(X_test)
```

3. __Train the model__

  * XGBoost is an optimized distributed gradient boosting library designed to be highly efficient, flexible and portable. It implements machine learning algorithms under the Gradient Boosting framework. XGBoost provides a parallel tree boosting (also known as GBDT, GBM) that solve many data science problems in a fast and accurate way.

```
from xgboost import XGBRegressor
xgb_reg = XGBRegressor()
xgb_reg.fit(X_train_s, y_train)
y_pred = xgb_reg.predict(X_test_s)
```

4. __Metric__

```
from sklearn.metrics import mean_absolute_error
mae = mean_absolute_error(y_test, y_pred)
```

```
Output
MAE: 4982.424870808919
```

5. __Fine-Tune the model__

```
from sklearn.model_selection import RandomizedSearchCV
import xgboost as xgb

param = {
    'learning_rate' : [0.01, 0.1, 0.15, 0.3, 0.5],
    'n_estimators' : [100, 500, 1000, 2000, 3000],
    'max_depth' : [3, 6, 9],
    'min_child_weight' : [1, 5, 10, 20],
    'reg_alpha' : [0.001, 0.01, 0.1],
    'reg_lambda' : [0.001, 0.01, 0.1]
}
model = XGBRegressor()
xgb_tune = RandomizedSearchCV(model, param_distributions = param,
                              n_iter = 100, scoring = 'neg_mean_absolute_error',
                              cv = 5)
       
xgb_search = xgb_tune.fit(X_train_s, y_train)
xgb_search.best_params_
y_pred_tune = xgb_search.predict(X_test_s)
mae_tune = mean_absolute_error(y_test, y_pred_tune)
```

```
Output
MAE: 3406.7991002400718
```
* After tuning, the MAE has decreasded from 4982 to 3407 which means the tuning process is useful. The best parameters of the model is obtained and this model will be used for features importance analysis.

6. __Feature importance__

```
from xgboost import XGBRegressor
xgb_reg_tuned = XGBRegressor(reg_lambda=0.01,
                             reg_alpha=0.001,
                             n_estimators=100,
                             min_child_weight=1,
                             max_depth=3,
                             learning_rate=0.1)
xgb_reg_tuned.fit(X_train_s, y_train)
plot_importance(xgb_reg_tuned, xlabel='Weight', ylabel=None)
```

<img src="https://github.com/yingchuwang/COVID-19-CASES-PREDICTION/blob/main/feature_importance.png" width="700">

## Conclusion
<img src="https://github.com/yingchuwang/COVID-19-CASES-PREDICTION/blob/main/feature_importance%202.png" width="700">

* The more people in the county and the larger the county is, the more COVID-19 cases is.
* Children under 5 and old people over 65 are very important for the cases number. 
* Black or African American is important for case number.
* Health insurance and sales retails are important.
* This model still needs improvement due to high MAE. 


