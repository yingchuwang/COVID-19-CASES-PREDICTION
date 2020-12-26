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

### Statistics data

Original data: https://github.com/yingchuwang/COVID-19-CASES-PREDICTION/blob/main/CA_census.csv

Steps:
1. Rotate the table. Make rows the county names and columns the census data.
2. Delete the 'County, California' in the county name.
3. Clean the columns title so that they can be accepted by MySQL.
4. Change the percentage format and currency format to number format.

### Case data

Original data: https://github.com/yingchuwang/COVID-19-CASES-PREDICTION/blob/main/CA_statewide_cases.csv

Steps:
1. Obtain the data only on 12/17/2020 using MySQL system.
2. Only keep the column 'totalcountconfirmed'.

### Combine data and export

After preprocessing, I combined the statistics data and cases data and exported it as a csv file: https://github.com/yingchuwang/COVID-19-CASES-PREDICTION/blob/main/data.csv
