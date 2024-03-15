# Cyclistic : Rider Trend Analysis
<p align="center">
  <img src="https://github.com/KarthikKovuri/Cyclistic_Trend_Analysis/assets/162425413/273facd0-45ea-4a0e-b8c0-c15a3bde1f80" alt="alt text" style="width:900px;height:450px;">
</p>

## Company Profile
Cyclistic is a bike-share program with over 5,800 bicycles and 600 docking stations, known for its inclusivity with options like reclining bikes and hand tricycles. Serving people with disabilities and those preferring non-standard bikes, it balances leisure and commuting needs. Approximately 30% of riders use the bikes for daily work commutes, showcasing its practicality beyond recreational use.

## Objective
The objective is to analyze Cyclistic's historical bike trip data to differentiate between annual members and casual riders, understand the reasons why casual riders might opt for a membership, and assess the potential influence of digital media on marketing tactics. Through this analysis, I aim to design effective marketing strategies aimed at converting casual riders into annual members, ultimately increasing membership uptake for Cyclistic.

## Data Source
The data utilized for this project originates from Motivate LLC, a real-world company, under the specified license agreement [here](https://divvybikes.com/data-license-agreement). I've amalgamated 12 months of data (from February 2023 to January 2024) from this source into a unified dataset for comprehensive analysis.

## Data Processing
**Data Cleaning**:
Initially, I consolidated data from 12 months into a single dataset, totaling 54,50,376 rows, each representing ride information. A meticulous review revealed missing 'start_station_names' and 'end_station_names' entries. For ride detail analysis, the complete dataset was utilized, while analysis involving station names excluded entries with null values, resulting in 41,30,450 rows.

**Data Preperation**: 
After incorporating derived columns such as 'ride_time', 'day', 'month', and 'time_range' into the existing dataset, the dataset now boasts improved attributes for thorough analysis.
Using the following Query: 

```sql
DROP table if exists `Cyclistic_ride_data_Feb23_to_Jan24.trip_data`;

create table `Cyclistic_ride_data_Feb23_to_Jan24.trip_data` as
select
  *,
  Round(timestamp_diff(ended_at,started_at,SECOND)/60,2) as ride_time, 
  Case
    when EXTRACT(DAYOFWEEK FROM started_at) = 1 then 'Sunday'
    when EXTRACT(DAYOFWEEK FROM started_at) = 2 then 'Monday'
    when EXTRACT(DAYOFWEEK FROM started_at) = 3 then 'Tuesday'
    when EXTRACT(DAYOFWEEK FROM started_at) = 4 then 'Wednesday'
    when EXTRACT(DAYOFWEEK FROM started_at) = 5 then 'Thurday'
    when EXTRACT(DAYOFWEEK FROM started_at) = 6 then 'Friday'
    when EXTRACT(DAYOFWEEK FROM started_at) = 7 then 'Saturday'
  End as day,
  CASE
    When Extract(MONTH from started_at) = 1 then 'January'
    When Extract(MONTH from started_at) = 2 then 'February'
    When Extract(MONTH from started_at) = 3 then 'March'
    When Extract(MONTH from started_at) = 4 then 'April'
    When Extract(MONTH from started_at) = 5 then 'May'
    When Extract(MONTH from started_at) = 6 then 'June'
    When Extract(MONTH from started_at) = 7 then 'July'
    When Extract(MONTH from started_at) = 8 then 'August'
    When Extract(MONTH from started_at) = 9 then 'September'
    When Extract(MONTH from started_at) = 10 then 'October'
    When Extract(MONTH from started_at) = 11 then 'November'
    When Extract(MONTH from started_at) = 12 then 'December'
  End as month,
  CASE
    WHEN EXTRACT(HOUR FROM started_at) >= 0 AND EXTRACT(HOUR FROM started_at) < 1 THEN '12AM - 1AM'
    WHEN EXTRACT(HOUR FROM started_at) >= 1 AND EXTRACT(HOUR FROM started_at) < 2 THEN '1AM - 2AM'
    WHEN EXTRACT(HOUR FROM started_at) >= 2 AND EXTRACT(HOUR FROM started_at) < 3 THEN '2AM - 3AM'
    WHEN EXTRACT(HOUR FROM started_at) >= 3 AND EXTRACT(HOUR FROM started_at) < 4 THEN '3AM - 4AM'
    WHEN EXTRACT(HOUR FROM started_at) >= 4 AND EXTRACT(HOUR FROM started_at) < 5 THEN '4AM - 5AM'
    WHEN EXTRACT(HOUR FROM started_at) >= 5 AND EXTRACT(HOUR FROM started_at) < 6 THEN '5AM - 6AM'
    WHEN EXTRACT(HOUR FROM started_at) >= 6 AND EXTRACT(HOUR FROM started_at) < 7 THEN '6AM - 7AM'
    WHEN EXTRACT(HOUR FROM started_at) >= 7 AND EXTRACT(HOUR FROM started_at) < 8 THEN '7AM - 8AM'
    WHEN EXTRACT(HOUR FROM started_at) >= 8 AND EXTRACT(HOUR FROM started_at) < 9 THEN '8AM - 9AM'
    WHEN EXTRACT(HOUR FROM started_at) >= 9 AND EXTRACT(HOUR FROM started_at) < 10 THEN '9AM - 10AM'
    WHEN EXTRACT(HOUR FROM started_at) >= 10 AND EXTRACT(HOUR FROM started_at) < 11 THEN '10AM - 11AM'
    WHEN EXTRACT(HOUR FROM started_at) >= 11 AND EXTRACT(HOUR FROM started_at) < 12 THEN '11AM - 12PM'
    WHEN EXTRACT(HOUR FROM started_at) >= 12 AND EXTRACT(HOUR FROM started_at) < 13 THEN '12PM - 1PM'
    WHEN EXTRACT(HOUR FROM started_at) >= 13 AND EXTRACT(HOUR FROM started_at) < 14 THEN '1PM - 2PM'
    WHEN EXTRACT(HOUR FROM started_at) >= 14 AND EXTRACT(HOUR FROM started_at) < 15 THEN '2PM - 3PM'
    WHEN EXTRACT(HOUR FROM started_at) >= 15 AND EXTRACT(HOUR FROM started_at) < 16 THEN '3PM - 4PM'
    WHEN EXTRACT(HOUR FROM started_at) >= 16 AND EXTRACT(HOUR FROM started_at) < 17 THEN '4PM - 5PM'
    WHEN EXTRACT(HOUR FROM started_at) >= 17 AND EXTRACT(HOUR FROM started_at) < 18 THEN '5PM - 6PM'
    WHEN EXTRACT(HOUR FROM started_at) >= 18 AND EXTRACT(HOUR FROM started_at) < 19 THEN '6PM - 7PM'
    WHEN EXTRACT(HOUR FROM started_at) >= 19 AND EXTRACT(HOUR FROM started_at) < 20 THEN '7PM - 8PM'
    WHEN EXTRACT(HOUR FROM started_at) >= 20 AND EXTRACT(HOUR FROM started_at) < 21 THEN '8PM - 9PM'
    WHEN EXTRACT(HOUR FROM started_at) >= 21 AND EXTRACT(HOUR FROM started_at) < 22 THEN '9PM - 10PM'
    WHEN EXTRACT(HOUR FROM started_at) >= 22 AND EXTRACT(HOUR FROM started_at) < 23 THEN '10PM - 11PM'
    WHEN EXTRACT(HOUR FROM started_at) >= 23 AND EXTRACT(HOUR FROM started_at) < 24 THEN '11PM - 12AM'
  END AS time_range
from `Cyclistic_ride_data_Feb23_to_Jan24.cyclistic_trip_data`;
```

## Query Result:
<details>
  <summary> Click to view SQL query results </summary>
  
![SS_table](https://github.com/KarthikKovuri/Cyclistic_Trend_Analysis/assets/162425413/d328b7ff-fd91-4c31-9dec-2e131fa3bd09)

</details>

## Data Analysis:
As part of my analysis, I began to grasp the holistic picture of the data. In this process, I began to develope multiple queries to unveil the true statistical landscape of the dataset.
### The overall rides taken by members and casuals are as below:

```sql
SELECT
 `member_non-member` as member_non_member,
 sum(electric_bike) as electiric_bike,
 sum(classic_bike) as calssic_bike,
 sum(docked_bike) as docked_bike,
 sum(total_bike_rides) as total_rides,
 Round(sum(total_bike_rides) / sum(sum(total_bike_rides)) OVER()*100,2) as ride_percentage
FROM
(
  SELECT
   member_casual as `member_non-member`,
   sum(CASE WHEN rideable_type = 'electric_bike' then 1 else 0 END) as electric_bike,
   sum(CASE WHEN rideable_type = 'classic_bike' then 1 else 0 END) as classic_bike,
   sum(CASE WHEN rideable_type = 'docked_bike' then 1 else 0 END) as docked_bike,
   sum(CASE WHEN rideable_type IN ('electric_bike','classic_bike','docked_bike') then 1 else 0 END) as total_bike_rides
  FROM `Cyclistic_ride_data_Feb23_to_Jan24.trip_data`
  Group by
   member_casual
)
GROUP BY 
 member_non_member
Order BY
  total_rides DESC;
```
### Querty Result:
| member_non_member | electric_bike | classic_bike | docked_bike | total_rides | ride_percentage |
|-------------------|---------------|--------------|-------------|-------------|-----------------|
| member            | 1,733,718     | 1,724,699    | 0           | 3,458,417   | 63.45           |
| casual            | 1,062,451     | 852,959      | 76,549      | 1,991,959   | 36.55           |
