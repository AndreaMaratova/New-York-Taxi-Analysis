
![Výstřižek](https://github.com/user-attachments/assets/027c812f-117d-4cb8-8b50-7bdac205b5a5)

# New York Taxi Analysis

## Problem definition
Predict the average money spent on taxi rides for each region of New York given day and hour.

This problem is a supervides regression problem. Supervised because we have the actual value of the value we are trying to predict and regression because what we are trying to predict is a continuous variable (as opposed to catgorical).

## Data sources

Data: https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page

Metdata: https://www.nyc.gov/assets/tlc/downloads/pdf/data_dictionary_trip_records_yellow.pdf

New York Weather Data: https://www.kaggle.com/datasets/aadimator/nyc-weather-2016-to-2022?resource=download&select=NYC_Weather_2016_2022.csv

## Data problems I fixed


*Negative total_amount values*: removed them because they are likely faulty data and there were not many of them.
*Zero values for total_amount*: same as negative values - removed.
![image](https://github.com/user-attachments/assets/529f309b-b8fe-4205-960f-b85d89e82240 "Zero and negative values")


*Too-high values for total_amount*: there were som hihg values for total_amount, some were close to $700.000. As there are unlikely values for a taxi ride, I decided to set an upper limit to $200. There were only 1.035 records with value over upper limit, which is not a great loss of inpormation. 
![image](https://github.com/user-attachments/assets/12a6153c-16f9-40fb-bd5c-d8cba1951f49 "Too high values")
