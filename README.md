# Major Power Outage Risk Analysis

**Name**: Spencer Goodwin

**Website Link**: https://spencerg00dwin.github.io/power_outages_analysis/


# Introduction
In this project, I analyzed a dataset of major power outages in the U.S. to explore patterns, causes, and impacts of these events. The dataset spans January 2000 to July 2016 and includes key attributes such as outage durations, causes, geographical regions, climate anomalies, and population data.

At first, I focused on cleaning the dataset and conducting exploratory data analysis to identify trends and anomalies. I specifically looked at climatic trends as a relation to power outage durations and averages. Along with this, visualizations such as the average outage duration by cause category, provided insights into the most impactful outage causes.

My model predict's outage durations based on both categorical and numerical features, such as climate region, cause category, El Niño/La Niña (ONI) anomoly index, and population. I started with a baseline Random Forest Regressor and later refined the model using hyperparameter tuning and feature selection techniques. I used evaluation metrics such as Mean Squared Error (MSE) to measure the performance of my model.

The original raw DataFrame contains 1534 rows, corresponding to 1534 outages, and 57 columns. I focused on these columns:
<table class="table table-striped table-bordered">
  <thead>
    <tr>
      <th>Column Name</th>
      <th>Definition</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>OBS</td>
      <td>Observation number or row identifier in the dataset</td>
    </tr>
    <tr>
      <td>YEAR</td>
      <td>Indicates the year when the outage event occurred</td>
    </tr>
    <tr>
      <td>MONTH</td>
      <td>Indicates the month when the outage event occurred</td>
    </tr>
    <tr>
      <td>U.S._STATE</td>
      <td>Represents all the states in the continental U.S.</td>
    </tr>
    <tr>
      <td>POSTAL.CODE</td>
      <td>Represents the postal code of the U.S. states</td>
    </tr>
    <tr>
      <td>NERC.REGION</td>
      <td>The North American Electric Reliability Corporation (NERC) regions involved in the outage event</td>
    </tr>
    <tr>
      <td>CLIMATE.REGION</td>
      <td>U.S. Climate regions as specified by National Centers for Environmental Information (nine climatically consistent regions in continental U.S.A.)</td>
    </tr>
    <tr>
      <td>ANOMALY.LEVEL</td>
      <td>Represents the oceanic El Niño/La Niña (ONI) index referring to the cold and warm episodes by season. It is estimated as a 3-month running mean of ERSST.v4 SST anomalies in the Niño 3.4 region (5°N to 5°S, 120–170°W)</td>
    </tr>
    <tr>
      <td>CLIMATE.CATEGORY</td>
      <td>Represents the climate episodes corresponding to the years. The categories—"Warm", "Cold" or "Normal" episodes of the climate are based on a threshold of ± 0.5 °C for the Oceanic Niño Index (ONI)</td>
    </tr>
    <tr>
      <td>CAUSE.CATEGORY</td>
      <td>Categories of all the events causing the major power outages</td>
    </tr>
    <tr>
      <td>CAUSE.CATEGORY.DETAIL</td>
      <td>Detailed description of the event categories causing the major power outages</td>
    </tr>
    <tr>
      <td>OUTAGE.DURATION</td>
      <td>Duration of outage events (in minutes)</td>
    </tr>
    <tr>
      <td>DEMAND.LOSS.MW</td>
      <td>Amount of peak demand lost during an outage event (in Megawatt) [but in many cases, total demand is reported]</td>
    </tr>
    <tr>
      <td>POPULATION</td>
      <td>Population in the U.S. state in a year</td>
    </tr>
    <tr>
      <td>OUTAGE.START</td>
      <td>Combines the date and time when the outage event started (as reported by the corresponding Utility in the region)</td>
    </tr>
    <tr>
      <td>OUTAGE.RESTORATION</td>
      <td>Combines the date and time when power was restored to all the customers (as reported by the corresponding Utility in the region)</td>
    </tr>
  </tbody>
</table>

# Cleaning and Exploratory Data Analysis
## Cleaning
1. First I needed to clean the data. I started by dropping severeal columns that were unnecessary for my model. Specifically, 'PCT_WATER_INLAND', 'PCT_WATER_TOT', 'PCT_LAND', 'AREAPCT_UC', 'AREAPCT_URBAN','POPDEN_RURAL', 'POPDEN_UC', 'POPDEN_URBAN', 'POPPCT_URBAN','PI.UTIL.OFUSA', 'UTIL.CONTRI', 'TOTAL.REALGSP', 'UTIL.REALGSP', 'PC.REALGSP.CHANGE', 'PC.REALGSP.REL', 'PC.REALGSP.USA', 'PC.REALGSP.STATE','IND.CUST.PCT', 'COM.CUST.PCT', 'RES.CUST.PCT', 'TOTAL.CUSTOMERS', 'IND.CUSTOMERS','COM.CUSTOMERS', 'RES.CUSTOMERS', 'IND.PERCEN', 'COM.PERCEN', 'RES.PERCEN', 'TOTAL.SALES','IND.SALES', 'COM.SALES', 'RES.SALES', 'TOTAL.PRICE', 'IND.PRICE', 'COM.PRICE', 'RES.PRICE','HURRICANE.NAMES', 'variables', 'POPPCT_UC', 'CUSTOMERS.AFFECTED'

2. I then went on to edit teh 'OUTAGE.START' and 'OUTAGE.RESTORATION' columns to be datetime objects. This was done by combining OUTAGE.START.DATE and OUTAGE.START.TIME into a single column, as well as OUTAGE.RESTORATION.DATE AND OUTAGE.RESTORATION.TIME into another column.

Here are the first few rows of my cleaned DataFrame:

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
This bar chart visualizes the average power outage duration for each cause category.
<iframe
  src="assets/average_outage_duration_by_cause.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
This bar chart displays the number of power outages across different climate regions.
<iframe
  src="assets/plot_two.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
This pie chart illustrates the proportion of power outages attributed to each climate category.
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

This pivot table highlights the average outage durations across various climate regions and cause categories. It reveals that certain combinations, such as East North Central with specific causes, exhibit significantly higher average durations. This further indicates regional and causal dependencies of outages. This variability underscores the importance of tailoring mitigation strategies to both the climate region and the underlying causes of outages.

<table class="table table-striped table-bordered">
  <thead>
    <tr>
      <th>Climate Region</th>
      <th>East North Central</th>
      <th>Northeast</th>
      <th>Central</th>
      <th>Northwest</th>
      <th>South</th>
      <th>Southeast</th>
      <th>Southwest</th>
      <th>West</th>
      <th>West North Central</th>
      <th>Overall Average</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Average Outage Duration</td>
      <td>26,435</td>
      <td>216</td>
      <td>322</td>
      <td>702</td>
      <td>296</td>
      <td>554</td>
      <td>114</td>
      <td>525</td>
      <td>61</td>
      <td>1,817</td>
    </tr>
    <tr>
      <td>Another Measure</td>
      <td>33,971</td>
      <td>14,630</td>
      <td>10,035</td>
      <td>1</td>
      <td>17,482</td>
      <td>NaN</td>
      <td>76</td>
      <td>6,155</td>
      <td>NaN</td>
      <td>13,484</td>
    </tr>
    <tr>
      <td>Third Measure</td>
      <td>2,376</td>
      <td>196</td>
      <td>346</td>
      <td>374</td>
      <td>326</td>
      <td>505</td>
      <td>266</td>
      <td>858</td>
      <td>24</td>
      <td>430</td>
    </tr>
    <tr>
      <td>Fourth Measure</td>
      <td>1</td>
      <td>881</td>
      <td>125</td>
      <td>73</td>
      <td>494</td>
      <td>NaN</td>
      <td>2</td>
      <td>215</td>
      <td>68</td>
      <td>201</td>
    </tr>
    <tr>
      <td>Fifth Measure</td>
      <td>733</td>
      <td>2,655</td>
      <td>1,410</td>
      <td>898</td>
      <td>1,164</td>
      <td>2,865</td>
      <td>2,275</td>
      <td>2,028</td>
      <td>440</td>
      <td>1,468</td>
    </tr>
    <tr>
      <td>Sixth Measure</td>
      <td>4,435</td>
      <td>4,430</td>
      <td>3,250</td>
      <td>4,838</td>
      <td>4,391</td>
      <td>2,663</td>
      <td>11,573</td>
      <td>2,928</td>
      <td>2,442</td>
      <td>3,900</td>
    </tr>
    <tr>
      <td>Seventh Measure</td>
      <td>2,610</td>
      <td>774</td>
      <td>2,695</td>
      <td>141</td>
      <td>866</td>
      <td>169</td>
      <td>329</td>
      <td>364</td>
      <td>NaN</td>
      <td>733</td>
    </tr>
    <tr>
      <td>Eighth Measure</td>
      <td>5,352</td>
      <td>2,992</td>
      <td>2,701</td>
      <td>1,284</td>
      <td>2,846</td>
      <td>2,218</td>
      <td>1,566</td>
      <td>1,628</td>
      <td>697</td>
      <td>2,631</td>
    </tr>
  </tbody>
</table>

## Imputation
I used probabilistic imputation to fill in missing values for OUTAGE.DURATION. This technique randomly samples from the observed values in the column to replace missing values, preserving the variability and distribution of the original data.

<iframe
  src="assets/imputation.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

# Framing
My prediction problem is to estimate outage duration (in minutes) based on pre-outage conditions, making this a regression task. The response variable OUTAGE.DURATION was chosen because it directly measures the severity and impact of a power outage which is critical for emergency planning. I evaluated my model using Mean Squared Error (MSE), as it penalizes large errors more heavily than alternatives like Mean Absolute Error (MAE). This focus is essential since large prediction errors for outage durations can lead to inadequate preparation or overestimation of resources.

To ensure the model is realistic and usable, I trained it only on features available at the time of prediction, such as climate region, cause category, population data, and the El Niño/La Niña (ONI) anomaly index. I excluded any information that would only become available after the outage began, like restoration times, to avoid data leakage. By selecting these features I aimed to build a model that can provide actionable predictions based on information known before an outage occurs. My approach started with a baseline Random Forest Regressor and improved through hyperparameter tuning and feature engineering focusing on maximizing accuracy while maintaining interpretability.

# Baseline Model
My model is a Random Forest Regressor that predicts power outage durations (in minutes) using two **nominal features**: `CAUSE.CATEGORY` and `CLIMATE.CATEGORY`, both encoded with **OneHotEncoder**. These features, representing the primary causes and climatic conditions of outages, were selected for their relevance to outage severity. The model achieved a baseline **MSE of `29,062,622.80`** and an **r^2 of `0.121`'**, indicating substantial prediction errors. While the model provides a foundation, its performance is limited by the small number of features, and further improvement is needed. Adding **quantitative features** like `ANOMALY.LEVEL` or `POPULATION` and tuning hyperparameters could enhance predictive accuracy and reduce error.

# Final Model
### Feature Selection and Justification

For my Final Model, I added two additional features: `ANOMALY.LEVEL` and `POPULATION`. These features were chosen because they are highly relevant to the data-generating process. `ANOMALY.LEVEL`, which represents deviations from typical climate patterns, captures the environmental conditions that often exacerbate power outages. For example, extreme weather events like storms or heatwaves can significantly increase outage durations. `POPULATION` accounts for the scale of the affected area, as more densely populated regions may experience longer outages due to increased recovery complexity or infrastructure strain. These features complement the existing categorical predictors (`CAUSE.CATEGORY` and `CLIMATE.REGION`) by adding quantitative context, which likely improves the model's ability to predict outage durations.

### Model and Hyperparameter Selection

I used a **Random Forest Regressor** for its robustness and ability to handle both categorical and numerical data effectively. The final hyperparameters were selected using **GridSearchCV** with 3-fold cross-validation. The best-performing hyperparameters were:
- **`n_estimators`**: `500` (number of trees in the forest)
- **`max_depth`**: `20` (maximum depth of each tree)
- **`min_samples_split`**: `2` (minimum samples required to split an internal node)
- **`max_features`**: `'sqrt'` (number of features considered for splitting at each node)

GridSearchCV systematically evaluated combinations of these hyperparameters, allowing the model to balance complexity and performance.

### Performance Improvement

The Final Model achieved an **MSE of `24,543,210.98`** and an **r^2 of `0.132`**, a significant improvement over the Baseline Model’s **MSE of `29,062,622.80`**. This reduction in error reflects the model’s enhanced ability to predict outage durations accurately by incorporating the additional quantitative features and optimizing hyperparameters. The performance boost aligns with the importance of `ANOMALY.LEVEL` and `POPULATION` in capturing the underlying variability in the data. Overall, the Final Model demonstrates better predictive accuracy and generalizability compared to the baseline.
