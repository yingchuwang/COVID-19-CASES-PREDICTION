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

1. Import the data into Jupyter notebook.

2. Seperate the data set into X_pre and y_pre, where X_pre is the statitics data and y_pre is the total confirmed cases number.

```
X_pre:  (58, 62)
y_pre:  (58,)
```
3. There are total 62 statitics items in the X_pre dataset. Except 'County', all the other columns are numerical. 
```
Index(['County', 'Population 2019', 'Population 2010',
       'Population percent change', 'Population Census 2010', 'Under 5 years',
       'Under 18 years', '65 years and over', 'Female persons', 'White alone',
       'Black or African American alone',
       'American Indian and Alaska Native alone', 'Asian alone',
       'Native Hawaiian and Other Pacific Islander alone', 'Two or More Races',
       'Hispanic or Latino', 'White alone, not Hispanic or Latino', 'Veterans',
       'Foreign born persons', 'Housing units',
       'Owner occupied housing unit rate',
       'Median value of owner occupied housing units',
       'Median selected monthly owner costs with a mortgage',
       'Median selected monthly owner costs without a mortgage',
       'Median gross rent', 'Building permits', 'Households',
       'Persons per household', 'Living in same house 1 year ago',
       'Language other than English spoken at home',
       'Households with a computer, percent',
       'Households with a broadband Internet subscription',
       'High school graduate or higher', 'Bachelor degree or higher',
       'With a disability, under age 65 years',
       'Persons without health insurance, under age 65 years',
       'In civilian labor force, total', 'In civilian labor force, female',
       'Total accommodation and food services sales',
       'Total health care and social assistance receipts or revenue',
       'Total manufacturers shipments', 'Total merchant wholesaler sales',
       'Total retail sales', 'Total retail sales per capita',
       'Mean travel time to work minutes',
       'Median household income in 2019 dollars',
       'Per capita income in past 12 months in 2019 dollars',
       'Persons in poverty', 'Total employer establishments',
       'Total employment', 'Total annual payroll',
       'Total employment, percent change', 'Total nonemployer establishments',
       'All firms', 'Men owned firms', 'Women owned firms',
       'Minority owned firms', 'Nonminority owned firms',
       'Veteran owned firms', 'Nonveteran owned firms',
       'Population per square mile', 'Land area in square miles'],
      dtype='object')
```
4. Correlation matrix is an effective tool to uncover linear relationship (Correlation) between two numerical features and the importance of a feature to the total confirmed cases number. 
![Correlation matrix](https://github.com/yingchuwang/COVID-19-CASES-PREDICTION/blob/main/correlation_matrix.png)
