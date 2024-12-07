# Major Power Outage Risk Analysis

**Name**: Spencer Goodwin

**Website Link**: https://spencerg00dwin.github.io/power_outages_analysis/


# Introduction
In this project, I analyzed a dataset of major power outages in the U.S. to explore patterns, causes, and impacts of these events. The dataset spans January 2000 to July 2016 and includes key attributes such as outage durations, causes, geographical regions, climate anomalies, and population data.

At first, I focused on cleaning the dataset and conducting exploratory data analysis to identify trends and anomalies. I specifically looked at climatic trends as a relation to power outage durations and averages. Along with this, visualizations such as the average outage duration by cause category, provided insights into the most impactful outage causes.

My model predict's outage durations based on both categorical and numerical features, such as climate region, cause category, El Niño/La Niña (ONI) anomoly index, and population. I started with a baseline Random Forest Regressor and later refined the model using hyperparameter tuning and feature selection techniques. I used evaluation metrics such as Mean Squared Error (MSE) to measure the performance of my model.

# Cleaning and Exploratory Data Analysis
## Cleaning
1. First I needed to clean the data. I started by dropping severeal columns that were unnecessary for my model. Specifically, 'PCT_WATER_INLAND', 'PCT_WATER_TOT', 'PCT_LAND', 'AREAPCT_UC', 'AREAPCT_URBAN','POPDEN_RURAL', 'POPDEN_UC', 'POPDEN_URBAN', 'POPPCT_URBAN','PI.UTIL.OFUSA', 'UTIL.CONTRI', 'TOTAL.REALGSP', 'UTIL.REALGSP', 'PC.REALGSP.CHANGE', 'PC.REALGSP.REL', 'PC.REALGSP.USA', 'PC.REALGSP.STATE','IND.CUST.PCT', 'COM.CUST.PCT', 'RES.CUST.PCT', 'TOTAL.CUSTOMERS', 'IND.CUSTOMERS','COM.CUSTOMERS', 'RES.CUSTOMERS', 'IND.PERCEN', 'COM.PERCEN', 'RES.PERCEN', 'TOTAL.SALES','IND.SALES', 'COM.SALES', 'RES.SALES', 'TOTAL.PRICE', 'IND.PRICE', 'COM.PRICE', 'RES.PRICE','HURRICANE.NAMES', 'variables', 'POPPCT_UC', 'CUSTOMERS.AFFECTED'

2. I then went on to edit teh 'OUTAGE.START' and 'OUTAGE.RESTORATION' columns to be datetime objects. This was done by combining OUTAGE.START.DATE and OUTAGE.START.TIME into a single column, as well as OUTAGE.RESTORATION.DATE AND OUTAGE.RESTORATION.TIME into another column.

Here are the first few rows of my cleaned DataFrame:
| OBS | YEAR | MONTH | U.S._STATE | POSTAL_CODE | NERC_REGION | CLIMATE_REGION | ANOMALY_LEVEL | CLIMATE_CATEGORY | CAUSE_CATEGORY | CAUSE_CATEGORY_DETAIL | OUTAGE_DURATION | DEMAND_LOSS_MW | POPULATION | OUTAGE_START | OUTAGE_RESTORATION |
|-----|------|-------|------------|-------------|-------------|----------------|---------------|-----------------|----------------|----------------------|-----------------|----------------|-------------|--------------|---------------------|
| 1 | 2011 | 7 | Minnesota | MN | MRO | East North Central | -0.3 | normal | severe weather | nan | 3060 | nan | 5348119 | 2011-07-01 17:00:00 | 2011-07-03 20:00:00 |
| 2 | 2014 | 5 | Minnesota | MN | MRO | East North Central | -0.1 | normal | intentional attack | vandalism | 1 | nan | 5457125 | 2014-05-11 18:38:00 | 2014-05-11 18:39:00 |
| 3 | 2010 | 10 | Minnesota | MN | MRO | East North Central | -1.5 | cold | severe weather | heavy wind | 3000 | nan | 5310903 | 2010-10-26 20:00:00 | 2010-10-28 22:00:00 |
| 4 | 2012 | 6 | Minnesota | MN | MRO | East North Central | -0.1 | normal | severe weather | thunderstorm | 2550 | nan | 5380443 | 2012-06-19 04:30:00 | 2012-06-20 23:00:00 |
| 5 | 2015 | 7 | Minnesota | MN | MRO | East North Central | 1.2 | warm | severe weather | nan | 1740 | 250 | 5489594 | 2015-07-18 02:00:00 | 2015-07-19 07:00:00 |


<table class="table table-striped table-bordered">
  <thead>
    <tr>
      <th>OBS</th>
      <th>YEAR</th>
      <th>MONTH</th>
      <th>U.S. STATE</th>
      <th>POSTAL CODE</th>
      <th>NERC REGION</th>
      <th>CLIMATE REGION</th>
      <th>ANOMALY LEVEL</th>
      <th>CLIMATE CATEGORY</th>
      <th>CAUSE CATEGORY</th>
      <th>CAUSE CATEGORY DETAIL</th>
      <th>OUTAGE DURATION</th>
      <th>DEMAND LOSS MW</th>
      <th>POPULATION</th>
      <th>OUTAGE START</th>
      <th>OUTAGE RESTORATION</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>1</td>
      <td>2011</td>
      <td>7</td>
      <td>Minnesota</td>
      <td>MN</td>
      <td>MRO</td>
      <td>East North Central</td>
      <td>-0.3</td>
      <td>normal</td>
      <td>severe weather</td>
      <td>nan</td>
      <td>3060</td>
      <td>nan</td>
      <td>5348119</td>
      <td>2011-07-01 17:00:00</td>
      <td>2011-07-03 20:00:00</td>
    </tr>
    <tr>
      <td>2</td>
      <td>2014</td>
      <td>5</td>
      <td>Minnesota</td>
      <td>MN</td>
      <td>MRO</td>
      <td>East North Central</td>
      <td>-0.1</td>
      <td>normal</td>
      <td>intentional attack</td>
      <td>vandalism</td>
      <td>1</td>
      <td>nan</td>
      <td>5457125</td>
      <td>2014-05-11 18:38:00</td>
      <td>2014-05-11 18:39:00</td>
    </tr>
    <tr>
      <td>3</td>
      <td>2010</td>
      <td>10</td>
      <td>Minnesota</td>
      <td>MN</td>
      <td>MRO</td>
      <td>East North Central</td>
      <td>-1.5</td>
      <td>cold</td>
      <td>severe weather</td>
      <td>heavy wind</td>
      <td>3000</td>
      <td>nan</td>
      <td>5310903</td>
      <td>2010-10-26 20:00:00</td>
      <td>2010-10-28 22:00:00</td>
    </tr>
    <tr>
      <td>4</td>
      <td>2012</td>
      <td>6</td>
      <td>Minnesota</td>
      <td>MN</td>
      <td>MRO</td>
      <td>East North Central</td>
      <td>-0.1</td>
      <td>normal</td>
      <td>severe weather</td>
      <td>thunderstorm</td>
      <td>2550</td>
      <td>nan</td>
      <td>5380443</td>
      <td>2012-06-19 04:30:00</td>
      <td>2012-06-20 23:00:00</td>
    </tr>
    <tr>
      <td>5</td>
      <td>2015</td>
      <td>7</td>
      <td>Minnesota</td>
      <td>MN</td>
      <td>MRO</td>
      <td>East North Central</td>
      <td>1.2</td>
      <td>warm</td>
      <td>severe weather</td>
      <td>nan</td>
      <td>1740</td>
      <td>250</td>
      <td>5489594</td>
      <td>2015-07-18 02:00:00</td>
      <td>2015-07-19 07:00:00</td>
    </tr>
  </tbody>
</table>


## Exploratory Data Analysis
### Univariate 
<iframe
  src="assets/average_outage_duration_by_cause.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

<iframe
  src="assets/plot_two.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
<iframe
  src="assets/plot_three.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Bivariate

<iframe
  src="assets/plot_four.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
<iframe
  src="assets/plot_five.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>