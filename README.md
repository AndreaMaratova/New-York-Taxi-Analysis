
![Výstřižek](https://github.com/user-attachments/assets/027c812f-117d-4cb8-8b50-7bdac205b5a5)

# New York Taxi Analysis

## Problem definition
Predict the average money spent on taxi rides for each region of New York given day and hour.

This problem is a supervised regression problem. Supervised because we have the actual value of the value we are trying to predict and regression because what we are trying to predict is a continuous variable (as opposed to categorical).

## Data sources

Data: https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page

Metdata: https://www.nyc.gov/assets/tlc/downloads/pdf/data_dictionary_trip_records_yellow.pdf

New York Weather Data: https://www.kaggle.com/datasets/aadimator/nyc-weather-2016-to-2022?resource=download&select=NYC_Weather_2016_2022.csv

I used the data for Feb 2019 (originally wanted Jan, but the data file was too large to upload to GitHub).

## Data problems I fixed


**Negative total_amount values**: removed them because they are likely faulty data and there were not many of them.
**Zero values for total_amount**: same as negative values - removed.

![image](https://github.com/user-attachments/assets/529f309b-b8fe-4205-960f-b85d89e82240 "Zero and negative values")


**Too-high values for total_amount**: there were some high values for total_amount, some were close to $700.000. As there are unlikely values for a taxi ride, I decided to set an upper limit to $200. There were only 1.035 records with value over upper limit, which is not a great loss of information. 

![image](https://github.com/user-attachments/assets/12a6153c-16f9-40fb-bd5c-d8cba1951f49 "Too high values")

## Original features of the model

Here is the list of features that can be used for model development that came with the original data: ["PULocationID", "transaction_date", "transaction_month", "transaction_day","transaction_hour", "trip_distance", "total_amount", "count_of_transactions"]

In [this document](https://www.nyc.gov/assets/tlc/downloads/pdf/data_dictionary_trip_records_yellow.pdf) you can find the meaning of given features.

## Feature engineering
I have added 3 sets of new features to the model. 

First were time-based features - whether it is a weekend or a national holiday.

Second were location-based features - we have a Location IDs per region but rhere is a higher level abstraction for regions called Boroughs. This information came from source of the main data. 

The last set was weather related data - I have downloaded data from [this website](https://www.kaggle.com/datasets/aadimator/nyc-weather-2016-to-2022?resource=download&select=NYC_Weather_2016_2022.csv).

Here is a list of all features used in the final model: ["PULocationID", "transaction_date", "transaction_month", "transaction_day", "transaction_hour", "trip_distance", "total_amount", "count_of_transactions", "transaction_week_day", "weekend", "is_holiday", "LocationID", "Borough", "temperature_2m (°C)", "precipitation (mm)", "rain (mm)", "cloudcover (%)", "windspeed_10m (km/h)"]

## The algorithms I tried and the results

I tried Decision Trees, Random Forest and Gradient Boosting. 

The benchmark model is a Decision Tree. In the benchmark model I only included the original features of the model as stated above. And on the normal models I used all original features plus the newly created ones. 

Performance results before tuning:
| Algorithm | MAE | RMSE | R2 |
| --- | --- | ---| --- |
| Benchmark Model | 8.617 | 13.830 | 0.289 |
| Decision Tree | 7.964 | 13.320 | 0.340 |
| Random Forest | 7.678 | 13.092 | 0.363 | 
| Gradient Boosting | 8.328 | 13.226 | 0.350 |

The Random Forest has the best values so I decided to tune this one.

I did two runs. I got the best values from model with this parameters: 
n_estimators: 400
min_sample_split: 40
min_samples_leaf: 10
max_features: None
max_depth: 300
Bootstrap: True

Mean test score: **0.404**, std_test_scores: **0.009**, time: **72.81 s**

Performance results after tuning:
| Algorithm | MAE | RMSE | R2 |
| --- | --- | ---| --- |
| Benchmark Model | 8.617 | 13.830 | 0.289 |
| Decision Tree | 7.964 | 13.320 | 0.340 |
| Random Forest | 7.678 | 13.092 | 0.363 | 
| Gradient Boosting | 8.328 | 13.226 | 0.350 |
| **Tuned Random Forest** | **7.287** | **12.398** | **0.428** |

Here is the True vs. Predicted value plot for the tuned Random Forest model. X-axis is the true values and y-axis the predicted values.
![image](https://github.com/user-attachments/assets/fd893e8a-f2f4-4dd4-b7ea-c6f65d0ec897)


## Next steps

As you can see form the plot above, there is still a plenty of room for improvement. 

- Limiting the regions/Boroughs inclued in this analysis. We could focus this model only for predicting taxi fare for Manhattan, Brooklyn and Queens regions by removing the rest of records. It would make this model more precise for there regions but since our main goal from the beginning was to predict fare in the whole New York City I used all the records.
  
| Borough | Value counts |
| --- | --- |
| Manhattan | 41021 |
| Brooklyn | 22236 |
| Queens | 20454 |
| Bronx | 9046 |
| Unknown | 672 |
| Staten Island | 305 |
| EWR | 272 |

- I would try to find better parameter combinantion for Random Forest model - not only the highest values but also the time is very important. If the performance is almost the same, sometimes is better to give preference to the faster settings. 








