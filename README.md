# Power Outages

by Stefan Boca (sboca@ucsd.edu)

## Introduction

In this project, we studied the factors that influence the severity of major power outages in the United States.

The dataset used in this project is available [here](https://engineering.purdue.edu/LASCI/research-data/outages/outagerisks). It has 1534 rows.

The columns relevant to this project are:

- `ANOMALY.LEVEL`: This represents the oceanic El Niño/La Niña index referring to the cold and warm episodes by season.
- `CAUSE.CATEGORY`: Categories of all the events causing major power outages.
- `CLIMATE.CATEGORY`: This represents the climate episodes corresponding to the years, with categories "Warm", "Cold" or “Normal”.
- `CUSTOMERS.AFFECTED`: The number of customers affected by the power outage event.
- `DEMAND.LOSS.MW`: Amount of peak demand lost during an outage event.
- `HURRICANE.NAMES`: If the outage is due to a hurricane, then the hurricane name is given by this column.
- `MONTH`: The month when the outage event occurred.
- `OUTAGE.DURATION`: Duration of the outage event, in minutes.
- `POPPCT_URBAN`: Percentage of the total population of the U.S. state represented by the urban population.
- `POPULATION`: Population in the U.S. state the outage occurred in.
- `TOTAL.PRICE`: Average mostly electricity price in the U.S. state.
- `U.S._STATE`: The state in which the outage event occurred.

## Data Cleaning and Exploratory Data Analysis

In the raw dataset, outage start and restoration times are recorded using
separate date and time columns. We combined each date-time pair into a single
datetime variable, stored in the OUTAGE.START and OUTAGE.RESTORATION columns.
This restructuring preserves the original reported information while making it
possible to analyze outage timing and compute durations.

Below, we show the head of the cleaned DataFrame:

| OBS | YEAR | MONTH | U.S.\_STATE | POSTAL.CODE | NERC.REGION | CLIMATE.REGION     | ANOMALY.LEVEL | CLIMATE.CATEGORY | OUTAGE.START.DATE   | OUTAGE.START.TIME | OUTAGE.RESTORATION.DATE | OUTAGE.RESTORATION.TIME | CAUSE.CATEGORY     | CAUSE.CATEGORY.DETAIL | HURRICANE.NAMES | OUTAGE.DURATION | DEMAND.LOSS.MW | CUSTOMERS.AFFECTED | RES.PRICE | COM.PRICE | IND.PRICE | TOTAL.PRICE |   RES.SALES |   COM.SALES |   IND.SALES | TOTAL.SALES | RES.PERCEN | COM.PERCEN | IND.PERCEN | RES.CUSTOMERS | COM.CUSTOMERS | IND.CUSTOMERS | TOTAL.CUSTOMERS | RES.CUST.PCT | COM.CUST.PCT | IND.CUST.PCT | PC.REALGSP.STATE | PC.REALGSP.USA | PC.REALGSP.REL | PC.REALGSP.CHANGE | UTIL.REALGSP | TOTAL.REALGSP | UTIL.CONTRI | PI.UTIL.OFUSA | POPULATION | POPPCT_URBAN | POPPCT_UC | POPDEN_URBAN | POPDEN_UC | POPDEN_RURAL | AREAPCT_URBAN | AREAPCT_UC | PCT_LAND | PCT_WATER_TOT | PCT_WATER_INLAND | OUTAGE.START        | OUTAGE.RESTORATION  |
| --: | ---: | ----: | :---------- | :---------- | :---------- | :----------------- | ------------: | :--------------- | :------------------ | :---------------- | :---------------------- | :---------------------- | :----------------- | :-------------------- | --------------: | --------------: | -------------: | -----------------: | --------: | --------: | --------: | ----------: | ----------: | ----------: | ----------: | ----------: | ---------: | ---------: | ---------: | ------------: | ------------: | ------------: | --------------: | -----------: | -----------: | -----------: | ---------------: | -------------: | -------------: | ----------------: | -----------: | ------------: | ----------: | ------------: | ---------: | -----------: | --------: | -----------: | --------: | -----------: | ------------: | ---------: | -------: | ------------: | ---------------: | :------------------ | :------------------ |
|   1 | 2011 |     7 | Minnesota   | MN          | MRO         | East North Central |          -0.3 | normal           | 2011-07-01 00:00:00 | 17:00:00          | 2011-07-03 00:00:00     | 20:00:00                | severe weather     | nan                   |             nan |            3060 |            nan |              70000 |      11.6 |      9.18 |      6.81 |        9.28 | 2.33292e+06 | 2.11477e+06 | 2.11329e+06 | 6.56252e+06 |    35.5491 |     32.225 |    32.2024 |       2308736 |        276286 |         10673 |         2595696 |      88.9448 |       10.644 |     0.411181 |            51268 |          47586 |        1.07738 |               1.6 |         4802 |        274182 |     1.75139 |           2.2 |    5348119 |        73.27 |     15.28 |         2279 |    1700.5 |         18.2 |          2.14 |        0.6 |  91.5927 |       8.40733 |          5.47874 | 2011-07-01 17:00:00 | 2011-07-03 20:00:00 |
|   2 | 2014 |     5 | Minnesota   | MN          | MRO         | East North Central |          -0.1 | normal           | 2014-05-11 00:00:00 | 18:38:00          | 2014-05-11 00:00:00     | 18:39:00                | intentional attack | vandalism             |             nan |               1 |            nan |                nan |     12.12 |      9.71 |      6.49 |        9.28 | 1.58699e+06 | 1.80776e+06 | 1.88793e+06 | 5.28423e+06 |    30.0325 |    34.2104 |    35.7276 |       2345860 |        284978 |          9898 |         2640737 |      88.8335 |      10.7916 |      0.37482 |            53499 |          49091 |        1.08979 |               1.9 |         5226 |        291955 |        1.79 |           2.2 |    5457125 |        73.27 |     15.28 |         2279 |    1700.5 |         18.2 |          2.14 |        0.6 |  91.5927 |       8.40733 |          5.47874 | 2014-05-11 18:38:00 | 2014-05-11 18:39:00 |
|   3 | 2010 |    10 | Minnesota   | MN          | MRO         | East North Central |          -1.5 | cold             | 2010-10-26 00:00:00 | 20:00:00          | 2010-10-28 00:00:00     | 22:00:00                | severe weather     | heavy wind            |             nan |            3000 |            nan |              70000 |     10.87 |      8.19 |      6.07 |        8.15 | 1.46729e+06 | 1.80168e+06 |  1.9513e+06 | 5.22212e+06 |    28.0977 |     34.501 |     37.366 |       2300291 |        276463 |         10150 |         2586905 |      88.9206 |       10.687 |     0.392361 |            50447 |          47287 |        1.06683 |               2.7 |         4571 |        267895 |     1.70627 |           2.1 |    5310903 |        73.27 |     15.28 |         2279 |    1700.5 |         18.2 |          2.14 |        0.6 |  91.5927 |       8.40733 |          5.47874 | 2010-10-26 20:00:00 | 2010-10-28 22:00:00 |
|   4 | 2012 |     6 | Minnesota   | MN          | MRO         | East North Central |          -0.1 | normal           | 2012-06-19 00:00:00 | 04:30:00          | 2012-06-20 00:00:00     | 23:00:00                | severe weather     | thunderstorm          |             nan |            2550 |            nan |              68200 |     11.79 |      9.25 |      6.71 |        9.19 | 1.85152e+06 | 1.94117e+06 | 1.99303e+06 | 5.78706e+06 |    31.9941 |    33.5433 |    34.4393 |       2317336 |        278466 |         11010 |         2606813 |      88.8954 |      10.6822 |     0.422355 |            51598 |          48156 |        1.07148 |               0.6 |         5364 |        277627 |     1.93209 |           2.2 |    5380443 |        73.27 |     15.28 |         2279 |    1700.5 |         18.2 |          2.14 |        0.6 |  91.5927 |       8.40733 |          5.47874 | 2012-06-19 04:30:00 | 2012-06-20 23:00:00 |
|   5 | 2015 |     7 | Minnesota   | MN          | MRO         | East North Central |           1.2 | warm             | 2015-07-18 00:00:00 | 02:00:00          | 2015-07-19 00:00:00     | 07:00:00                | severe weather     | nan                   |             nan |            1740 |            250 |             250000 |     13.07 |     10.16 |      7.74 |       10.43 | 2.02888e+06 | 2.16161e+06 | 1.77794e+06 | 5.97034e+06 |    33.9826 |    36.2059 |    29.7795 |       2374674 |        289044 |          9812 |         2673531 |      88.8216 |      10.8113 |     0.367005 |            54431 |          49844 |        1.09203 |               1.7 |         4873 |        292023 |      1.6687 |           2.2 |    5489594 |        73.27 |     15.28 |         2279 |    1700.5 |         18.2 |          2.14 |        0.6 |  91.5927 |       8.40733 |          5.47874 | 2015-07-18 02:00:00 | 2015-07-19 07:00:00 |

<iframe src="assets/s2_f1.html" width="800" height="450" frameborder="0"></iframe>

The histogram of outage duration shows a strongly skewed distribution.
Most power outages last for a relatively short amount of time, while a small
number of events have extremely long durations. This pattern suggests that
severe, prolonged outages are rare but disproportionately impactful.

<iframe src="assets/s2_f2.html" width="800" height="450" frameborder="0"></iframe>

This scatter plot shows no clear linear relationship between the two variables.
Most outage events are clustered near low values for both duration and
customers affected, indicating that the majority of outages are relatively
small in scale. However, a small number of extreme events affect many customers
and/or last for long periods.

| CAUSE.CATEGORY                | OUTAGE.DURATION |
| :---------------------------- | --------------: |
| fuel supply emergency         |           13484 |
| severe weather                |         3883.99 |
| equipment failure             |         1816.91 |
| public appeal                 |         1468.45 |
| system operability disruption |          728.87 |
| intentional attack            |          429.98 |
| islanding                     |         200.545 |

The table shows clear differences in average outage duration across cause
categories. Fuel supply emergencies and severe weather events are associated
with substantially longer outages, while causes such as intentional attacks and
islanding tend to result in shorter durations on average.

## Assessment of Missingness

I do not believe that any column in this dataset is NMAR. For the variables
with missingness (seen below), the missing values are more plausibly explained
by limitations in reporting, data availability, or event circumstances. For
example, missing outage duration values are likely due to incomplete utility
reports, not because the true duration is inherently long or short.

Let's explore the missingness of the `DEMAND.LOSS.MW` column.

## Hypothesis Testing

In this section, we run a permutation test to determine if the `OUTAGE.DURATION` column depends on whether `CLIMATE.CATEGORY` is `"cold"`.

**Null hypothesis:** Mean outage duration is the same in warm and non-warm years.
**Alternate hypothesis:** Mean outage duration is not the same in cold years.

The absolute difference of means statistic was used. We obtained a p-value of
0.89 (>0.5), which is high. We fail to reject the null hypothesis. We conclude
that it is likely that mean outage duration has the same distribution,
regardless of whether the climate is cold or not.

## Framing a Prediction Problem

## Baseline Model

## Final Model

## Fairness Analysis
