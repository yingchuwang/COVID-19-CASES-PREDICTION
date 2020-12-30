# COVID-19-CASES-PREDICTION
## Table of contents 
* [Background](#background)
* [Data Collection](#data-collection)
* [Data Preprocessing](#data-preprocessing)
* [Data Processing](#data-processing)

## Background
A novel coronavirus is a new coronavirus that has not been previously identified. The virus causing coronavirus disease 2019 (COVID-19), is not the same as the coronaviruses that commonly circulate among humans and cause mild illness, like the common cold [1](https://www.cdc.gov/coronavirus/2019-nCoV/index.html). 

California is reporting the fourth-highest rate of new COVID-19 cases per capita in the country, and the state's hospitals are overwhelmed and considering rationing care. Several regions have gone under new stay-at-home orders in recent weeks [2](https://www.sfgate.com/bayarea/article/California-COVID-lockdown-cases-deaths-businesses-15819841.php).

At this time, learning about California statistics and their relations with COVID-19 cases numbers is very meaningful. It gives us understanding about why some regions have more cases and others don't. It will also give us ideas about how to prevent COVID-19 spread more efficiently.  

## Data Collection

The datasets are selected and downloaded from [Census Bureau website](https://www.census.gov/quickfacts/fact/table/US/PST045219) and [Cailifornia Open Data Portal](https://data.ca.gov/dataset/covid-19-cases). One dataset is the statistics of all California couties in 2019 and the other one is the COVID-19 cases of all counties from 3/17/2020 to 12/17/2020.

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
<img src="https://github.com/yingchuwang/COVID-19-CASES-PREDICTION/blob/main/correlation_matrix.png" width="700" height="700">

4. __Identify the correlation coefficient between features and target__. First 10 rows are shown as follow.

Viarables                                                   |totalcountconfirmed
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

1. Remove redundant features

Per the analysis above, the redundant features are removed. The column 'County' is also removed because the name will not affect the result.

2. Fill missing values

Viarables                                                   |Missing value
------------------------------------------------------------|--------------
Total employment, percent change	                           |1
Native Hawaiian and Other Pacific Islander alone	         |1

There are only two viarables have missing values and each one has only one missing value. I replace the missing value with the mean value. After removing redundant features and filling with missing data, the total number of columns has reduced to 27. The information below shows that no missing value in the dataset.

```
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 58 entries, 0 to 57
Data columns (total 27 columns):
 #   Column                                                Non-Null Count  Dtype  
---  ------                                                --------------  -----  
 0   Population 2019                                       58 non-null     int64  
 1   Population percent change                             58 non-null     float64
 2   Under 5 years                                         58 non-null     float64
 3   65 years and over                                     58 non-null     float64
 4   Female persons                                        58 non-null     float64
 5   White alone                                           58 non-null     float64
 6   Black or African American alone                       58 non-null     float64
 7   American Indian and Alaska Native alone               58 non-null     float64
 8   Asian alone                                           58 non-null     float64
 9   Native Hawaiian and Other Pacific Islander alone      58 non-null     float64
 10  Two or More Races                                     58 non-null     float64
 11  Hispanic or Latino                                    58 non-null     float64
 12  Foreign born persons                                  58 non-null     float64
 13  Owner occupied housing unit rate                      58 non-null     float64
 14  Median gross rent                                     58 non-null     int64  
 15  Living in same house 1 year ago                       58 non-null     float64
 16  Households with a computer, percent                   58 non-null     float64
 17  High school graduate or higher                        58 non-null     float64
 18  With a disability, under age 65 years                 58 non-null     float64
 19  Persons without health insurance, under age 65 years  58 non-null     float64
 20  In civilian labor force, total                        58 non-null     float64
 21  Total retail sales per capita                         58 non-null     int64  
 22  Mean travel time to work minutes                      58 non-null     float64
 23  Persons in poverty                                    58 non-null     float64
 24  Total employment, percent change                      58 non-null     float64
 25  Population per square mile                            58 non-null     float64
 26  Land area in square miles                             58 non-null     float64
dtypes: float64(24), int64(3)
memory usage: 12.4 KB
```
## Modelling

1. Use _train_test_split_ from _sklearn_ to split the dataset into training data and test data.

2. Use _XGBRgressor_ from _xgboost_ to fit the training data. XGBoost is an optimized distributed gradient boosting library designed to be highly efficient, flexible and portable. It implements machine learning algorithms under the Gradient Boosting framework. XGBoost provides a parallel tree boosting (also known as GBDT, GBM) that solve many data science problems in a fast and accurate way. 

3. 
