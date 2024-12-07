# Major Power Outage Risk Analysis

**Name**: Spencer Goodwin

**Website Link**: https://spencerg00dwin.github.io/power_outages_analysis/


# Introduction
In this project, I analyzed a dataset of major power outages in the U.S. to explore patterns, causes, and impacts of these events. The dataset spans January 2000 to July 2016 and includes key attributes such as outage durations, causes, geographical regions, climate anomalies, and population data.

At first, I focused on cleaning the dataset and conducting exploratory data analysis to identify trends and anomalies. I specifically looked at climatic trends as a relation to power outage durations and averages. Along with this, visualizations such as the average outage duration by cause category, provided insights into the most impactful outage causes.

My model predict's outage durations based on both categorical and numerical features, such as climate region, cause category, El Niño/La Niña (ONI) anomoly index, and population. I started with a baseline Random Forest Regressor and later refined the model using hyperparameter tuning and feature selection techniques. I used evaluation metrics such as Mean Squared Error (MSE) to measure the performance of my model.

# Cleaning and EDA
1. First I needed to clean the data. I started by dropping severeal columns that were unnecessary for my model. Specifically, 'PCT_WATER_INLAND', 'PCT_WATER_TOT', 'PCT_LAND', 'AREAPCT_UC', 'AREAPCT_URBAN','POPDEN_RURAL', 'POPDEN_UC', 'POPDEN_URBAN', 'POPPCT_URBAN','PI.UTIL.OFUSA', 'UTIL.CONTRI', 'TOTAL.REALGSP', 'UTIL.REALGSP', 'PC.REALGSP.CHANGE', 'PC.REALGSP.REL', 'PC.REALGSP.USA', 'PC.REALGSP.STATE','IND.CUST.PCT', 'COM.CUST.PCT', 'RES.CUST.PCT', 'TOTAL.CUSTOMERS', 'IND.CUSTOMERS','COM.CUSTOMERS', 'RES.CUSTOMERS', 'IND.PERCEN', 'COM.PERCEN', 'RES.PERCEN', 'TOTAL.SALES','IND.SALES', 'COM.SALES', 'RES.SALES', 'TOTAL.PRICE', 'IND.PRICE', 'COM.PRICE', 'RES.PRICE','HURRICANE.NAMES', 'variables', 'POPPCT_UC', 'CUSTOMERS.AFFECTED'

2. I then went on to edit teh 'OUTAGE.START' and 'OUTAGE.RESTORATION' columns to be datetime objects. This was done by combining OUTAGE.START.DATE and OUTAGE.START.TIME into a single column, as well as OUTAGE.RESTORATION.DATE AND OUTAGE.RESTORATION.TIME into another column.

Here are the first few rows of my cleaned DataFrame:
| OBS | YEAR | MONTH | U.S._STATE | POSTAL.CODE | NERC.REGION | CLIMATE.REGION     | ANOMALY.LEVEL | CLIMATE.CATEGORY | CAUSE.CATEGORY     | CAUSE.CATEGORY.DETAIL | OUTAGE.DURATION | DEMAND.LOSS.MW | POPULATION | OUTAGE.START         | OUTAGE.RESTORATION    |
|-----|------|-------|------------|-------------|-------------|--------------------|---------------|------------------|--------------------|-----------------------|-----------------|----------------|------------|---------------------|---------------------|
| 1   | 2011 | 7.0   | Minnesota  | MN          | MRO         | East North Central | -0.3          | normal           | severe weather     | NaN                   | 3060.0          | NaN            | 5348119    | 2011-07-01 17:00:00 | 2011-07-03 20:00:00 |
| 2   | 2014 | 5.0   | Minnesota  | MN          | MRO         | East North Central | -0.1          | normal           | intentional attack | vandalism             | 1.0             | NaN            | 5457125    | 2014-05-11 18:38:00 | 2014-05-11 18:39:00 |
| 3   | 2010 | 10.0  | Minnesota  | MN          | MRO         | East North Central | -1.5          | cold             | severe weather     | heavy wind            | 3000.0          | NaN            | 5310903    | 2010-10-26 20:00:00 | 2010-10-28 22:00:00 |
| 4   | 2012 | 6.0   | Minnesota  | MN          | MRO         | East North Central | -0.1          | normal           | severe weather     | thunderstorm          | 2550.0          | NaN            | 5380443    | 2012-06-19 04:30:00 | 2012-06-20 23:00:00 |
| 5   | 2015 | 7.0   | Minnesota  | MN          | MRO         | East North Central | 1.2           | warm             | severe weather     | NaN                   | 1740.0          | 250.0          | 5489594    | 2015-07-18 02:00:00 | 2015-07-19 07:00:00 |
