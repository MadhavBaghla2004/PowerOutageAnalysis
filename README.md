## Contributors:
1. [Madhav Baghla](https://github.com/MadhavBaghla2004)
2. [Nadine Marcus](https://github.com/nadinemarcus)

## Introduction:

What question are we trying to answer? <br>
Through this project, we want to address **how characteristics of major power outages have changed over time and if there is a visible trend**.

Why should you care about our project? <br>
Power outages are significant events that impact critical infrastructure, affecting various sectors such as healthcare, transportation, communication, and commerce. By studying major power outages, our project can contribute valuable insights into improving the resilience and reliability of the power grid, thus enhancing the stability of critical infrastructure.

Imagine a world with no light! <br>


<img src="https://64.media.tumblr.com/259ada11b519f626aafec0b2c8ac4b49/tumblr_p4okyyrj371qfr6udo1_500.gif"> 

Where did we get the data from? <br>
Our [dataset](https://engineering.purdue.edu/LASCI/research-data/outages) includes the major outages witnessed by different states in the continental U.S. Besides major outages, this data contains information on geographical location of the outages, regional climatic information, land-use characteristics, electricity consumption patterns and economic characteristics of the states affected by the outages. 

Number of Rows: 1541 <br>
Number of Columns: 4

| SNo | Column Name | Description |
|----------|----------|-------------|
| 1 | CAUSE.CATEGORY | Categories of all the events causing the major power outages   |
| 2    | OUTAGE.DURATION    |  Duration of outage events (in minutes) |
| 3    | CLIMATE.CATEGORY    | This represents the climate episodes corresponding to the years. The categories—“Warm”, “Cold” or “Normal” episodes of the climate are based on a threshold of ± 0.5 °C for the Oceanic Niño Index (ONI) |
| 4 | CUSTOMERS.AFFECTED | Number of customers affected by the power outage event |

## Data Cleaning and Exploratory Data Analysis:
For Data Cleaning, we 
1. **Column Selection**: We extracted the columns that pertained to our analysis: `'CAUSE.CATEGORY'`, `'OUTAGE.DURATION'`, `'CLIMATE.REGION'`, `'CLIMATE.CATEGORY'`, `'YEAR'`, and `'CUSTOMERS.AFFECTED'`
2. **Cleaning Missing Values**: We dropped rows with all missing values so only meaningful data is used for our analysis and that there are no empty rows.
   - For further cleaning, I organized the DataFrame so that they were grouped by each individual categorical variable and dropped rows without any data for those subcategories using <br>
```outages_cleaned.groupby(['CLIMATE.REGION','CAUSE.CATEGORY','CLIMATE.CATEGORY']).agg('mean').dropna(how=all)```
3. **Replacing invalid data**: We replaced negative or zero values in the `'OUTAGE.DURATION'` column to NaN (not a number) as they aren't valid durations for this dataset. 

| CAUSE.CATEGORY     |   OUTAGE.DURATION | CLIMATE.REGION     | CLIMATE.CATEGORY   |   CUSTOMERS.AFFECTED |   YEAR |
|:-------------------|------------------:|:-------------------|:-------------------|---------------------:|-------:|
| severe weather     |              3060 | East North Central | normal             |                70000 |   2011 |
| intentional attack |                 1 | East North Central | normal             |                  nan |   2014 |
| severe weather     |              3000 | East North Central | cold               |                70000 |   2010 |
| severe weather     |              2550 | East North Central | normal             |                68200 |   2012 |
| severe weather     |              1740 | East North Central | warm               |               250000 |   2015 |
### Plots:
#### Univariate
<iframe
  src="assets/fig_univariate.html"
  width="600"
  height="500"
  frameborder="0"
></iframe>

It's hard to understand how this distribution is laid out, so let's also create a log univariate graph to get a better understanding of how the data is distributed.

##### Log Univariate
<iframe
  src="assets/fig_log_univariate.html"
  width="600"
  height="500"
  frameborder="0"
></iframe>

##### Bivariate
<iframe
  src="assets/fig_bivariate.html"
  width="600"
  height="500"
  frameborder="0"
></iframe>

#### Interesting Aggregates Pivot Table
The pivot table offers a clearer perspective on the median outage duration by examining the interplay between the cause of the outage, `'CAUSE.CATEGORY'`, the climate conditions, `'CLIMATE.CATEGORY'`, and the scale of customers affected, `'CUSTOMERS.AFFECTED'`. 

The multivariate analysis underscores the variability in outage durations depending on these factors, with certain patterns emerging, such as severe weather outages tending to last longer as the number of affected customers rises, particularly in cold climates. It also highlights that large-scale outages, especially those affecting over a million customers, result in extended durations. In contrast, smaller-scale outages, like those due to intentional attacks or islanding, generally have shorter durations, suggesting that they are often localized and potentially easier to resolve. 

|   CAUSE.CATEGORY, CLIMATE.CATEGORY    |   < 1000 |   1,000 - 10,000 |   10,000 - 100,000 |   100,000 - 1,000,000 |   > 1,000,000 |
|:------------------------------------|----------------:|--------------------:|----------------------:|------------------------:|-------------------:|
| ('equipment failure', 'cold')       |             200 |                 nan |                 376   |                   182   |                nan |
| ('equipment failure', 'normal')     |             125 |                 nan |                 161.5 |                   752.5 |                nan |
| ('equipment failure', 'warm')       |             909 |                 nan |                 221   |                    64   |                nan |
| ('fuel supply emergency', 'normal') |            2891 |                 nan |                 nan   |                   nan   |                nan |
| ('intentional attack', 'cold')      |             nan |                  60 |                 nan   |                   nan   |                nan |

## Assessment of Missingness:

The `'OUTAGES.DURATION'` column could likely be **NMAR** given that missingness could be due to 'invalid' or negligently brief outage duration times. We can contrast this missingness with the values of the `'CUSTOMERS.AFFECTED'` column to see if there are any consistencies or trends that can determine how big these outages are or if they could be described as negligent or not enough data.

When conducting a permutation test to assess the missingness association between the `'OUTAGE.DURATION'` and the `'CUSTOMERS.AFFECTED'` in the form of shuffled a new column: `'CUSTOMER.BINS'`, a column created to organize it into different `bins` of `[0, 1000, 10000, 100000, 1000000, np.inf]`. 

`Observed Statistic: 32044.474390936633
P-value: 0.01`

Based off of the code, the observed statistic indicates that the standard deviation between the missing outage durations and the number of customers affected is approximately 32,044, and the p-value is 0.01, indicating that there is a 1% chance of observing a standard deviation as extreme as this by chance if the null hypothesis were true.

This indicates that the `'OUTAGES.DURATION'` column could likely be **MAR** with the `'CUSTOMERS.AFFECTED'` column.

<iframe
  src="assets/fig_cust_vs_duration_missing.html"
  width="600"
  height="500"
  frameborder="0"
></iframe>

## Hypothesis Testing:

Null Hypothesis: The median outage duration is the same for major power outages caused by different factors, occurring across various climate regions, affecting different numbers of customers.

Alternative Hypothesis: The median outage duration differs for major power outages caused by different factors, occurring across various climate regions, affecting different numbers of customers. 

Test Statistic: Median outage duration


## Prediction Problem:

Predict the duration of major power outages based on various factors such as cause category, climate region, and the number of customers affected. It is a **regression** problem.
We want to predict the **OUTAGE.DURATION** column in our dataset

## Baseline Model:

## Final Model:

## Fairness Analysis:










