# COVID-19-CASES-PREDICTION
## Table of contents 
* [Background](#background)
* [Data Collection](#data-collection)
* [Preprocessing](#preprocessing)

## Background
A novel coronavirus is a new coronavirus that has not been previously identified. The virus causing coronavirus disease 2019 (COVID-19), is not the same as the coronaviruses that commonly circulate among humans and cause mild illness, like the common cold [1]. 

California is reporting the fourth-highest rate of new COVID-19 cases per capita in the country, and the state's hospitals are overwhelmed and considering rationing care. Several regions have gone under new stay-at-home orders in recent weeks [2].

At this time, learning about California statistics and their relations with COVID-19 cases numbers is very meaningful. It gives us understanding about why some regions have more cases and others don't. It will also give us ideas about how to prevent COVID-19 spread more efficiently.  

References:

[1] https://www.cdc.gov/coronavirus/2019-nCoV/index.html

[2] https://www.sfgate.com/bayarea/article/California-COVID-lockdown-cases-deaths-businesses-15819841.php

## Data Collection

The statistics for all counties of California were downloaded from Census Bureau (version 7/1/2019) [3].

The COVID-19 case data was downloaded from California Open Data Portal (version 12/17/2020) [4].

[3] https://www.census.gov/quickfacts/fact/table/US/PST045219

[4] https://data.ca.gov/dataset/covid-19-cases

## Preprocessing

Statistics data - [Data link](https://github.com/yingchuwang/COVID-19-CASES-PREDICTION/blob/main/CA_census.csv)

1. Rotate the table. Make rows the county names and columns the census data.
2. Delete the 'County, California' in the county name.
3. Clean the columns title so that they can be accepted by MySQL.
4. Deal with the cell format and change all format to 'number' format.
5. Delete the 'FIPS CODE' column.
6. Delete the cell value with 'D','F','C'.

Case data - [Data link](https://github.com/yingchuwang/COVID-19-CASES-PREDICTION/blob/main/CA_statewide_cases.csv)

1. Obtain the data only on 12/17/2020 using MySQL system.
2. Only keep the column 'totalcountconfirmed'.

Combine data and export - [Data link](https://github.com/yingchuwang/COVID-19-CASES-PREDICTION/blob/main/data.csv)

1. After preprocessing, combined the statistics data and cases data and exported it as a csv file.

## Understanding data

1. Import the dataset 'data_pre' into Jupyter notebook.

2. Show the shape of the dataset

```
data_pre:  (58, 63)
```
3. Look close to the dataset. All the columns are numerical except 'County'. There are total 63 columns in it. Nine columns have missing values. They are 'Native Hawaiian and Other Pacific Islander alone', 'Total accommodation and food services sales', 'Total health care and social assistance receipts or revenue', 'Total manufacturers shipments', 'Total merchant wholesaler sales', 'Total employment, percent change', 'Men owned firms', 'Minority owned firm', 'Veteran owned firms'. I will deal with these missing values later.
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
 10  Black or African American alone                              58 non-null     float64
 11  American Indian and Alaska Native alone                      58 non-null     float64
 12  Asian alone                                                  58 non-null     float64
 13  Native Hawaiian and Other Pacific Islander alone             57 non-null     float64
 14  Two or More Races                                            58 non-null     float64
 15  Hispanic or Latino                                           58 non-null     float64
 16  White alone, not Hispanic or Latino                          58 non-null     float64
 17  Veterans                                                     58 non-null     int64  
 18  Foreign born persons                                         58 non-null     float64
 19  Housing units                                                58 non-null     int64  
 20  Owner occupied housing unit rate                             58 non-null     float64
 21  Median value of owner occupied housing units                 58 non-null     int64  
 22  Median selected monthly owner costs with a mortgage          58 non-null     int64  
 23  Median selected monthly owner costs without a mortgage       58 non-null     int64  
 24  Median gross rent                                            58 non-null     int64  
 25  Building permits                                             58 non-null     int64  
 26  Households                                                   58 non-null     int64  
 27  Persons per household                                        58 non-null     float64
 28  Living in same house 1 year ago                              58 non-null     float64
 29  Language other than English spoken at home                   58 non-null     float64
 30  Households with a computer, percent                          58 non-null     float64
 31  Households with a broadband Internet subscription            58 non-null     float64
 32  High school graduate or higher                               58 non-null     float64
 33  Bachelor degree or higher                                    58 non-null     float64
 34  With a disability, under age 65 years                        58 non-null     float64
 35  Persons without health insurance, under age 65 years         58 non-null     float64
 36  In civilian labor force, total                               58 non-null     float64
 37  In civilian labor force, female                              58 non-null     float64
 38  Total accommodation and food services sales                  55 non-null     float64
 39  Total health care and social assistance receipts or revenue  55 non-null     float64
 40  Total manufacturers shipments                                42 non-null     float64
 41  Total merchant wholesaler sales                              39 non-null     float64
 42  Total retail sales                                           58 non-null     int64  
 43  Total retail sales per capita                                58 non-null     int64  
 44  Mean travel time to work minutes                             58 non-null     float64
 45  Median household income in 2019 dollars                      58 non-null     int64  
 46  Per capita income in past 12 months in 2019 dollars          58 non-null     int64  
 47  Persons in poverty                                           58 non-null     float64
 48  Total employer establishments                                58 non-null     int64  
 49  Total employment                                             58 non-null     int64  
 50  Total annual payroll                                         58 non-null     int64  
 51  Total employment, percent change                             57 non-null     float64
 52  Total nonemployer establishments                             58 non-null     int64  
 53  All firms                                                    58 non-null     int64  
 54  Men owned firms                                              57 non-null     float64
 55  Women owned firms                                            58 non-null     int64  
 56  Minority owned firms                                         56 non-null     float64
 57  Nonminority owned firms                                      58 non-null     int64  
 58  Veteran owned firms                                          56 non-null     float64
 59  Nonveteran owned firms                                       58 non-null     int64  
 60  Population per square mile                                   58 non-null     float64
 61  Land area in square miles                                    58 non-null     float64
 62  totalcountconfirmed                                          58 non-null     int64  
dtypes: float64(38), int64(24), object(1)
memory usage: 28.7+ KB
```
4. Correlation matrix is an effective method to uncover the linear relationship (correlation) between two numerical features. It also shows the importance of a feature to the total confirmed cases number. 
![Correlation matrix](https://github.com/yingchuwang/COVID-19-CASES-PREDICTION/blob/main/correlation_matrix.png)
