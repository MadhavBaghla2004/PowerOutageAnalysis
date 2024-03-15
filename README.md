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

## Assessment of Missingness:

## Hypothesis Testing:

Null Hypothesis: The median outage duration is the same for major power outages caused by different factors, occurring across various climate regions, affecting different numbers of customers.

Alternative Hypothesis: The median outage duration differs for major power outages caused by different factors, occurring across various climate regions, affecting different numbers of customers. 

Test Statistic: Median outage duration


## Prediction Problem:

Predict the duration of major power outages based on various factors such as cause category, climate region, and the number of customers affected. It is a **regression** problem.
We want to predict the OUTAGE.DURATION column in our dataset

## Baseline Model:

## Final Model:

## Fairness Analysis:










