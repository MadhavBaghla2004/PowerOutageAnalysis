## Contributors:
1. [Madhav Baghla](https://github.com/MadhavBaghla2004)
2. [Nadine Marcus](https://github.com/nadinemarcus)

<img src="https://i.insider.com/602c4bcd5847630018323626?width=1200&format=jpeg"> 

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
For Data Cleaning, 
1. **Column Selection**: We extracted the columns that pertained to our analysis: `'CAUSE.CATEGORY'`, `'OUTAGE.DURATION'`, `'CLIMATE.REGION'`, `'CLIMATE.CATEGORY'`, `'YEAR'`, and `'CUSTOMERS.AFFECTED'`
2. **Cleaning Missing Values**: We dropped rows with all missing values so only meaningful data is used for our analysis and that there are no empty rows.
   - For further cleaning, we organized the DataFrame so that they were grouped by each individual categorical variable and dropped rows without any data for those subcategories using <br>
```outages_cleaned.groupby(['CLIMATE.REGION','CAUSE.CATEGORY','CLIMATE.CATEGORY'])``` <br>
```.agg('mean').dropna(how=all)```
4. **Replacing invalid data**: We replaced negative or zero values in the `'OUTAGE.DURATION'` column to NaN (not a number) as they aren't valid durations for this dataset. For categorical columns, I replaced missing values or invalid values with 'NaN' so the data would be unbiased.
   

|CAUSE.CATEGORY   |OUTAGE.DURATION |CLIMATE.REGION      |CLIMATE.CATEGORY |CUSTOMERS.AFFECTED |YEAR |
|:------------|-------------|-------------|-------------|-------------|-----|
| severe weather   |            3060 | East North Central  | normal           |              70000 | 2011 |
| intentional attack |              1 | East North Central | normal           |                nan | 2014 |
| severe weather   |            3000 | East North Central  | cold             |              70000 | 2010 |
| severe weather   |            2550 | East North Central  | normal           |              68200 | 2012 |
| severe weather   |            1740 | East North Central  | warm             |             250000 | 2015 |




### Plots:
#### Univariate
<iframe
  src="assets/fig_univariate.html"
  width="600"
  height="500"
  frameborder="0"
></iframe>
The graph shows the distribution of `'OUTAGE.DURATION'` in minutes, zoomed in to show the values before 10,000, as the larger values were dispersed and almost unvisible in the graph. The majority of outages were less than 2,000 minutes.

It's hard to understand how this distribution is laid out, so we also created a log univariate graph to get a better understanding of how the data is distributed.

##### Log Univariate
<iframe
  src="assets/fig_log_univariate.html"
  width="600"
  height="500"
  frameborder="0"
></iframe>

This histogram presents the log-transformed distribution of power outage durations, with a dashed green line indicating the average, and a hover feature that reveals the count of outages for each bin. The visualization employs a logarithmic scale to more clearly show the spread and central tendency of the data.

##### Bivariate
<iframe
  src="assets/fig_bivariate.html"
  width="600"
  height="500"
  frameborder="0"
></iframe>
This graph demonstrates the relationship between the `'CUSTOMERS.AFFECTED'` and `'OUTAGE.DURATION'` values. The graph is zoomed in to a limited axis of a duration up to 10,000 minutes and 750,000 customers affected to exclude extreme outliers and get a clearer vision of the distribution of the data.

The graph also includes information from the `'CAUSE.CATEGORY'` through the scatter plot's colors and the `'CLIMATE.CATEGORY'` when you hover over a data point.

This graph shows that there is no upfront correlation between the two columns, but the majority of the larger power outage durations were caused by severe weather.

#### Interesting Aggregates Pivot Table
The pivot table offers a clearer perspective on the median outage duration by examining the interplay between the cause of the outage, `'CAUSE.CATEGORY'`, the climate conditions, `'CLIMATE.CATEGORY'`, and the scale of customers affected, `'CUSTOMERS.AFFECTED'`. 

The multivariate analysis underscores the variability in outage durations depending on these factors, with certain patterns emerging, such as severe weather outages tending to last longer as the number of affected customers rises, particularly in cold climates. It also highlights that large-scale outages, especially those affecting over a million customers, result in extended durations. In contrast, smaller-scale outages, like those due to intentional attacks or islanding, generally have shorter durations, suggesting that they are often localized and potentially easier to resolve. 

|   CAUSE.CATEGORY, CLIMATE.CATEGORY    |   < 1000 |   1,000 - 10,000 |   10,000 - 100,000 |   100,000 - 1,000,000 |   > 1,000,000 |
|------------------------------------|----------------|--------------------|----------------------|------------------------|-------------------|
| ('equipment failure', 'cold')       |             200 |                 nan |                 376   |                   182   |                nan |
| ('equipment failure', 'normal')     |             125 |                 nan |                 161.5 |                   752.5 |                nan |
| ('equipment failure', 'warm')       |             909 |                 nan |                 221   |                    64   |                nan |
| ('fuel supply emergency', 'normal') |            2891 |                 nan |                 nan   |                   nan   |                nan |
| ('intentional attack', 'cold')      |             nan |                  60 |                 nan   |                   nan   |                nan |

## Assessment of Missingness:

The `'OUTAGES.DURATION'` column could likely be **NMAR** given that missingness could be due to 'invalid' or negligently brief outage duration times. We can contrast this missingness with the values of the `'CUSTOMERS.AFFECTED'` column to see if there are any consistencies or trends that can determine how big these outages are or if they could be described as negligent or not enough data.

We conducted a permutation test to assess the missingness association between the `'OUTAGE.DURATION'` and the `'CUSTOMERS.AFFECTED'` in the form of shuffled a new column: `'CUSTOMER.BINS'`, a column created to organize it into different `bins` of `[0, 1000, 10000, 100000, 1000000, np.inf]`:

**Observed Statistic**: 90223.66605616607

**P-value**: 0.006

This p-value indicates the probability of observing a test statistic as extreme as, or more extreme than, the observed statistic under the null hypothesis, which in this case assumes no association between the missingness of outage duration and the number of customers affected. A P-value of 0.006 suggests that there is a statistically significant association between the two variables, as it's below the significance level of 0.05.

This indicates that the `'OUTAGES.DURATION'` column could likely be **MAR** with the `'CUSTOMERS.AFFECTED'` column.

<iframe
  src="assets/fig_cust_vs_duration_missing_zoom.html"
  width="600"
  height="500"
  frameborder="0"
></iframe>

<iframe
  src="assets/fig_empirical_dist_missingness.html"
  width="600"
  height="500"
  frameborder="0"
></iframe>


## Hypothesis Testing:

**Null Hypothesis**: The median outage duration is independent of the cause of the outage, the climate region where it occurs, and the number of customers affected.

**Alternative Hypothesis**: The median outage varies depending on the cause of the outage, the climate region where it occurs, and the number of customers affected. Specific factors such as severe weather, regions like the Northwest and Southeast, and the number of customers affected play a significant role in these variations.

**Test Statistic**: Median outage duration

<iframe
  src="assets/fig_duration_over_time.html"
  width="600"
  height="500"
  frameborder="0"
></iframe>

<iframe
  src="assets/fig_cause_vs_duration.html"
  width="600"
  height="500"
  frameborder="0"
></iframe>

<iframe
  src="assets/fig_climate_region_vs_duration.html"
  width="600"
  height="500"
  frameborder="0"
></iframe>

The computed permutation test to test these hypothesis arrived at these results:

**Observed Statistic**: 198131.0

**P-value**: 0.0201

The observed statistic represents the aggregate deviation of median outage durations within each category combination from `'CAUSE.CATEGORY'`, `'CLIMATE.CATEGORY'`, and `'CUSTOMER.BIN'`, from the overall median outage duration across the entire dataset.

Given that we have chosen a significance level of 0.05, this p-value indicates that *we fail to reject the null hypothesis*. Under the null hypothesis of there being no difference in median outage durations across different categories, there is an 8.5% chance of observing an aggregate deviation as extreme as the one observed in the data due to random chance.

## Prediction Problem:
The model is to predict the duration of major power outages based on the cause of the outage, the climate region where it occurs, and the number of customers affected. This is a regression problem, as we are predicting a continuous outcome (outage duration in minutes).

#### Response Variable
**Response Variable**: `'OUTAGE.DURATION'`

Understanding the duration of power outages can help utility companies in planning and resource allocation to mitigate the effects of outages. Predicting outage duration can also aid in effectively communicating with customers regarding outage resolutions.

#### Evaluation Metric
**Chosen Metric**: Root Mean Squared Error (RMSE)

RMSE is a suitable metric for regression problems as it measures the average magnitude of the error between predicted and actual values, providing an estimate of how far predictions are from the actual outage durations. It penalizes larger errors more than Mean Absolute Error (MAE), making it more sensitive to outliers, which is valuable in a context where large deviations from actual durations are more impactful. Additionally, RMSE is in the same unit as the target variable, making it intuitively easier to interpret.

#### Model and Features
**Model**: Linear Regression

**Features**: `'CAUSE.CATEGORY'`, `'CLIMATE.REGION'`, `'CUSTOMERS.AFFECTED'`

These features are chosen based on the assumption that the cause of the outage and the climate region could influence the duration due to varying severity and response strategies. The number of customers affected is also a proxy for the outage's scale, which could impact duration.

#### Data Preprocessing
**Numerical Features**: `'CUSTOMERS.AFFECTED'` is standardized and missing values are imputed with the median, as it is less sensitive to outliers than the mean.

**Categorical Features**: `'CAUSE.CATEGORY'` and `'CLIMATE.REGION'` are one-hot encoded to transform them into a format suitable for linear regression. Missing values are imputed with the most frequent category within each feature, assuming that the most common occurrences are likely to represent typical cases.


## Baseline Model:

For the baseline model of our prediction task on power outage durations, we've chosen to use a combination of quantitative and nominal features based on the data analyzed above. The quantitative feature in our model is `'CUSTOMERS.AFFECTED'`, which represents the number of customers impacted by a power outage. The nominal features are `'CAUSE.CATEGORY'` and `'CLIMATE.REGION'`, which detail the cause of the outage and the climate region where it occurred, respectively.

To handle the preprocessing of these features, we've implemented a sklearn pipeline that takes care of both numerical and categorical data. For the numerical feature `'CUSTOMERS.AFFECTED'`, we've used a SimpleImputer with a median strategy to fill in any missing values, ensuring that our model has complete data to learn from. For the categorical features `'CAUSE.CATEGORY'` and `'CLIMATE.REGION'`, we've applied a two-step process with a SimpleImputer set to the most frequent strategy to fill missing values, followed by OneHotEncoder to transform these nominal features into a format suitable for modeling.

The RandomForestRegressor from sklearn serves as our model choice for this baseline setup, renowned for its robustness and capability to handle complex, non-linear relationships without extensive hyperparameter tuning. We've trained this model using a random state of 42 for reproducibility.

Upon evaluating our baseline model on the test set, we achieved a Baseline Root Mean Squared Error (RMSE) of *6904.40021995283*. This metric quantifies the average magnitude of the errors between the predicted and actual outage durations, giving us a sense of how well our model is performing.

#### Assessment of Model Performance:
Given the RMSE value, the performance of our baseline model can be considered as an initial benchmark. While the RandomForestRegressor is a powerful algorithm capable of capturing complex patterns in the data, the choice of features and their preprocessing could be further optimized for better performance. The current RMSE indicates the average error in the predictions, which, depending on the context of the application, can be evaluated for its adequacy. The model will require more feature engineering since the graphs and data throughout this project haven't concluded anything concrete for us to implement.

## Final Model:
For the final model in our power outage duration prediction task, we aimed to improve upon the baseline model by introducing a refined feature set and optimizing our model's hyperparameters. The model employs the GradientBoostingRegressor, a powerful ensemble method that builds sequential trees to minimize errors. The features utilized include 'CAUSE.CATEGORY', 'CLIMATE.REGION', and 'CUSTOMERS.AFFECTED', reflecting the cause of outage, the geographical climate, and the scale of customer impact respectively.

#### Feature Engineering and Selection:

**Quantile Transformation on `'CUSTOMERS.AFFECTED'`**: To address potential skewness and scale the numerical data more uniformly, we applied a QuantileTransformer. This transformation makes our model less sensitive to outliers in the number of customers affected by outages, ensuring that extreme values do not disproportionately influence the model's learning.

**OneHot Encoding for Categorical Features**: The `'CAUSE.CATEGORY'` and `'CLIMATE.REGION'` features were one-hot encoded to convert categorical information into a machine-readable format. This encoding expands our model's capability to recognize patterns associated with specific outage causes and climate regions.

#### Hyperparameter Tuning:

To fine-tune the model, we utilized GridSearchCV to explore a range of values for the GradientBoostingRegressor's hyperparameters, including `n_estimators`, `max_depth`, and `learning_rate`. This systematic approach allowed us to identify the optimal settings that enhance the model's performance by balancing bias and variance, preventing overfitting while ensuring robust predictive capability.

*n_estimators* was varied to determine the ideal number of sequential trees that contribute to reducing the model's error.
*max_depth* controlled the complexity of the trees, ensuring they are sufficiently detailed to capture underlying patterns without becoming overly complex.
*learning_rate* adjusted the contribution of each tree, fine-tuning the balance between learning speed and the risk of overfitting.

### Performance and Improvements:

The hyperparameters that performed best were identified through a comprehensive GridSearchCV process:

`regressor__learning_rate`: 0.01 - This learning rate was found to be optimal as it ensures gradual learning. A slower learning rate can lead to better generalization and prevents the model from overfitting on the training data.

`regressor__max_depth`: 3 - A depth of 3 strikes a good balance between model complexity and generalization. Deeper trees could capture more intricate patterns but risk overfitting, while shallower trees might underfit.

`regressor__n_estimators`: 100 - The choice of 100 trees was deemed sufficient for this model to achieve a balance between prediction accuracy and computational efficiency. Adding more trees beyond this point yielded diminishing returns on performance improvement.

The GradientBoostingRegressor's ability to iteratively correct errors from previous trees played a pivotal role in enhancing prediction accuracy.

The final model achieved an RMSE of 6503.701, which represents the average deviation between the predicted and actual outage durations. While this is an improvement from the baseline model's RMSE of 6904.400, the enhancement is relatively modest.

#### Reflection on Limited Improvement:

The limited improvement in the model's performance despite feature engineering and hyperparameter optimization suggests a few potential reasons:

**Complex Nature of Outage Durations**: The duration of power outages can be influenced by a multitude of factors, many of which may not be captured fully by the available features. External variables such as emergency response efficiency, infrastructure age, and specific damage details play a critical role in outage resolution but were not included in the model.

**Data Quality and Availability**: The model's ability to predict outage durations could be hindered by the quality of the available data. Missing values, inaccuracies, or the lack of granularity in the cause categorization and climate region information could limit the model's learning capability.

**Inherent Predictive Limitations**: Some aspects of power outage durations may simply be too random or influenced by unforeseeable events, making them inherently difficult to predict accurately with the data at hand.

**Conclusion**:
While the final model demonstrates an improvement over the baseline, its modest performance gain highlights the challenges in predicting power outage durations. The selected hyperparameters and feature engineering efforts provided some benefits, but the complexity of the underlying problem and potential data limitations suggest that further improvements may require additional data sources, more sophisticated models, or a reevaluation of the prediction task itself.

## Fairness Analysis:

Group X: CAUSE.CATEGORY <br>
Group Y: Prediction error (Absolute Error) <br>
Evaluation Metric: Mean Absolute Error <br>
Null Hypothesis : There is no significant difference in prediction error distributions between different cause categories. <br>
Alternative Hypothesis : There is a significant difference in prediction error distributions between at least one pair of cause categories. <br>
Test Statistic: F-statistic from the ANOVA test <br>
Significance Level: α = 0.05 (chosen significance level) <br>
Resulting p-value: 7.928293976019978e-14 <br>

**Conclusion: With a p-value significantly less than the chosen significance level (p < 0.05), we reject the null hypothesis. Therefore, we conclude that there are significant differences in prediction error distributions between at least one pair of cause categories.**










