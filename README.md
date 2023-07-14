# Cyclistic_case_study

Data Source: [divvy_tripdata](https://divvy-tripdata.s3.amazonaws.com/index.html)

## Context

A bike-share program that features more than 5,800 bicycles and 600 docking stations. Cyclistic sets itself apart by also offering reclining bikes, hand tricycles, and cargo bikes, making bike-share more inclusive to people with disabilities and riders who can’t use a standard two-wheeled bike. The majority of riders opt for traditional bikes; about 8% of riders use the assistive options. Cyclistic users are more likely to ride for leisure, but about 30% use them to commute to work each day.

Until now, Cyclistic’s marketing strategy relied on building general awareness and appealing to broad consumer segments. One approach that helped make these things possible was the flexibility of its pricing plans: single-ride passes, full-day passes, and annual memberships. Customers who purchase single-ride or full-day passes are referred to as casual riders. Customers who purchase annual memberships are Cyclistic members.

Cyclistic’s finance analysts have concluded that annual members are much more profitable than casual riders. Although the pricing flexibility helps Cyclistic attract more customers, Moreno (the director of marketing and my manager) believes that maximizing the number of annual members will be key to future growth. Rather than creating a marketing campaign that targets all-new customers, Moreno believes there is a very good chance to convert casual riders into members. She notes that casual riders are already aware of the Cyclistic program and have chosen Cyclistic for their mobility needs.

Moreno has set a clear goal: Design marketing strategies aimed at converting casual riders into annual members. In order to do that, however, the marketing analyst team needs to better understand how annual members and casual riders differ, why casual riders would buy a membership, and how digital media could affect their marketing tactics. Moreno and her team are interested in analyzing the Cyclistic historical bike trip data to identify trends.

## Data Organization
There are 12 files with naming convention of YYYYMM-divvy-tripdata and each file includes information for one month, such as the ride id, bike type, start time, end time, start station, end station, start location, end location, and whether the rider is a member or not. The corresponding column names are ride_id, rideable_type, started_at, ended_at, start_station_name, start_station_id, end_station_name, end_station_id, start_lat, start_lng, end_lat, end_lng and member_casual.

## Process
BigQuery is used to combine the various datasets into one dataset and clean it.
Reason:
A worksheet can only have 1,048,576 rows in Microsoft Excel because of its inability to manage large amounts of data. Because the Cyclistic dataset has more than 5.6 million rows, it is essential to use a platform like BigQuery that supports huge volumes of data.

## Combining the Data
12 csv files are uploaded as tables in the dataset '2022_tripdata'. Another table named "combined_data" is created, containing 5,667,717 rows of data for the entire year.

## Data Exploration
Before cleaning the data, I am familiarizing myself with the data to find the inconsistencies.

Observations:

The table below shows all column names and their data types. The ride_id column is our primary key.

![image](https://github.com/Thdor/Cyclistic_case_study/assets/117071722/b29cd1b0-6e06-48f9-9bed-ae05a61f9198)

The following table shows the number of null values in each column.

![image](https://github.com/Thdor/Cyclistic_case_study/assets/117071722/1ac506df-c678-47d2-8a51-f94ae1207640)

Note that some columns have a same number of missing values. This may be due to missing information in the same row i.e. station's name and id for the same station and latitude and longitude for the same ending station.

As ride_id has no null values, let's use it to check for duplicates.

![image](https://github.com/Thdor/Cyclistic_case_study/assets/117071722/f17e4b7a-15db-4569-8fc8-68d9cc1b810d)

There are no duplicate rows in the data.

All ride_id values have a length of 16 so no need to clean it.

There are 3 unique types of bikes(rideable_type) in our data.

![image](https://github.com/Thdor/Cyclistic_case_study/assets/117071722/cb793821-54ea-4a3a-9dbf-f60bbd558079)

The started_at and ended_at shows start and end time of the trip in YYYY-MM-DD hh:mm:ss UTC format. New column ride_length can be created to find the total trip duration. There are 5360 trips which has duration longer than a day and 122283 trips having less than a minute duration or having end time earlier than start time so need to remove them. Other columns day_of_week and month can also be helpful in analysis of trips at different times in a year.

Total of 833064 rows have both start_station_name and start_station_id missing which needs to be removed.

Total of 892742 rows have both end_station_name and end_station_id missing which needs to be removed.

Total of 5858 rows have both end_lat and end_lng missing which needs to be removed.

member_casual column has 2 uniqued values as member or casual rider.

![image](https://github.com/Thdor/Cyclistic_case_study/assets/117071722/7701ff47-85ef-476f-a57f-400eedf05678)

Columns that need to be removed are start_station_id and end_station_id as they do not add value to analysis of our current problem. Longitude and latitude location columns may not be used in analysis but can be used to visualise a map.

## Data Cleaning

All the rows having missing values are deleted.
3 more columns ride_length for duration of the trip, day_of_week and month are added.
Trips with duration less than a minute and longer than a day are excluded.
Total 1,375,912 rows are removed in this step.
