# Cyclistic : Rider Trend Analysis
<p align="center">
  <img src="https://github.com/KarthikKovuri/Cyclistic_Trend_Analysis/assets/162425413/273facd0-45ea-4a0e-b8c0-c15a3bde1f80" alt="alt text" style="width:900px;height:450px;">
</p>

## Company Profile
Cyclistic is a bike-share program with over 5,800 bicycles and 600 docking stations, known for its inclusivity with options like reclining bikes and hand tricycles. Serving people with disabilities and those preferring non-standard bikes, it balances leisure and commuting needs. Approximately 30% of riders use the bikes for daily work commutes, showcasing its practicality beyond recreational use.

## Objective
The objective is to analyze Cyclistic's historical bike trip data to differentiate between annual members and casual riders, understand the reasons why casual riders might opt for a membership, and assess the potential influence of digital media on marketing tactics. Through this analysis, I aim to design effective marketing strategies aimed at converting casual riders into annual members, ultimately increasing membership uptake for Cyclistic.

## Data Source
The data utilized for this project originates from Motivate LLC, a real-world company, under the specified license agreement [here](https://divvybikes.com/data-license-agreement). I've consolidated 12 months of data (from February 2023 to January 2024) from this source into a unified dataset for comprehensive analysis.

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

### Overall rides taken by members and casuals are as below:

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
### Query Result:
| member_non_member | electric_bike | classic_bike | docked_bike | total_rides | ride_percentage |
|-------------------|---------------|--------------|-------------|-------------|-----------------|
| member            | 1,733,718     | 1,724,699    | 0           | 3,458,417   | 63.45           |
| casual            | 1,062,451     | 852,959      | 76,549      | 1,991,959   | 36.55           |

### A breakdown of number of rides per day.

```sql
SELECT
  day,
  sum(`non-member`) as total_non_member,
  sum(member) as total_member,
  sum(total_rides_day) as total_rides,
  Round((sum(total_rides_day) / sum(sum(total_rides_day)) OVER ())*100,2) as ride_percentage_day
FROM
(
  SELECT
    day,
    sum(CASE WHEN member_casual = "casual" then 1 else 0 END) as `non-member`,
    sum(CASE WHEN member_casual = "member" then 1 else 0 END) as `member`,
    sum(CASE WHEN member_casual IN ("casual", "member") then 1 else 0 END) as total_rides_day
  FROM `Cyclistic_ride_data_Feb23_to_Jan24.trip_data`
  GROUP BY day  
) as ride_per_day
GROUP BY
  day
ORDER BY
  total_rides DESC;
```
### Query Result: 
| day        | total_non_member | total_member | total_rides | ride_percentage_day |
|------------|------------------|--------------|-------------|---------------------|
| Saturday   | 398,510          | 444,460      | 842,970     | 15.47               |
| Thurday    | 262,353          | 561,090      | 823,443     | 15.11               |
| Wednesday  | 240,794          | 560,454      | 801,248     | 14.7                |
| Friday     | 300,509          | 495,927      | 796,436     | 14.61               |
| Tuesday    | 237,418          | 542,837      | 780,255     | 14.32               |
| Sunday     | 324,237          | 383,537      | 707,774     | 12.99               |
| Monday     | 228,138          | 470,112      | 698,250     | 12.81               |

### Monthly distribution of ride counts.

```sql
SELECT
  month,
  sum(non_member) as total_non_member,
  sum(member) as total_member,
  sum(total_monthly_rides) as total_monthly_rides,
  Round((sum(total_monthly_rides) / sum(sum(total_monthly_rides)) OVER ())*100,2) as monthly_rides_percentage
FROM
(
SELECT
 month,
 sum(CASE WHEN member_casual = "casual" then 1 else 0 END) as non_member,
 sum(CASE WHEN member_casual = "member" then 1 else 0 END) as member,
 sum(CASE WHEN member_casual IN ("casual", "member") then 1 else 0 END) as total_monthly_rides
FROM `Cyclistic_ride_data_Feb23_to_Jan24.trip_data`
GROUP BY
  month
)
Group By
 month
ORDER BY
 total_monthly_rides DESC;
```

### Query Result:
| month     | total_non_member | total_member | total_monthly_rides | monthly_rides_percentage |
|-----------|------------------|--------------|---------------------|-------------------------|
| August    | 311,130          | 460,563      | 771,693             | 14.16                   |
| July      | 331,358          | 436,292      | 767,650             | 14.08                   |
| June      | 301,230          | 418,388      | 719,618             | 13.2                    |
| September | 261,635          | 404,736      | 666,371             | 12.23                   |
| May       | 234,181          | 370,646      | 604,827             | 11.1                    |
| October   | 177,071          | 360,042      | 537,113             | 9.85                    |
| April     | 147,285          | 279,305      | 426,590             | 7.83                    |
| November  | 98,392           | 264,126      | 362,518             | 6.65                    |
| March     | 62,201           | 196,477      | 258,678             | 4.75                    |
| February  | 43,016           | 147,429      | 190,445             | 3.49                    |
| January   | 24,460           | 120,413      | 144,873             | 2.66                    |

### Hourly breakdown of rides throughout the day.

```sql
select
 time_range,
 sum(non_member) as non_member,
 sum(member) as member,
 sum(rides_per_hour) as rides_per_hour,
 round(sum(rides_per_hour) / (sum(sum(rides_per_hour)) OVER())*100,2) as monthly_rides_per_hour
from
(
select 
 time_range,
 sum(case when member_casual = 'casual' then 1 else 0 end) as non_member,
 sum(case when member_casual = 'member' then 1 else 0 end) as member,
 sum(case when member_casual IN ('member','casual') then 1 else 0 end) as rides_per_hour
from `Cyclistic_ride_data_Feb23_to_Jan24.trip_data`
group by 
 time_range
)
GROUP BY
 time_range
ORDER BY
 non_member DESC;
```
### Query Result:
| time_range   | non_member | member  | rides_per_hour | monthly_rides_per_hour |
|--------------|------------|---------|----------------|------------------------|
| 5PM - 6PM    | 193,947    | 369,117 | 563,064        | 10.33                  |
| 4PM - 5PM    | 176,658    | 313,727 | 490,385        | 9                      |
| 6PM - 7PM    | 167,644    | 293,365 | 461,009        | 8.46                   |
| 3PM - 4PM    | 153,831    | 231,780 | 385,611        | 7.07                   |
| 2PM - 3PM    | 137,851    | 189,635 | 327,486        | 6.01                   |
| 1PM - 2PM    | 132,220    | 186,290 | 318,510        | 5.84                   |
| 12PM - 1PM   | 126,580    | 187,475 | 314,055        | 5.76                   |
| 7PM - 8PM    | 123,917    | 207,921 | 331,838        | 6.09                   |
| 11AM - 12PM  | 106,821    | 165,354 | 272,175        | 4.99                   |
| 8PM - 9PM    | 89,402     | 144,767 | 234,169        | 4.3                    |
| 10AM - 11AM  | 83,595     | 139,458 | 223,053        | 4.09                   |
| 9PM - 10PM   | 74,970     | 112,134 | 187,104        | 3.43                   |
| 8AM - 9AM    | 67,841     | 230,130 | 297,971        | 5.47                   |
| 9AM - 10AM   | 67,360     | 154,957 | 222,317        | 4.08                   |
| 10PM - 11PM  | 66,282     | 83,654  | 149,936        | 2.75                   |
| 7AM - 8AM    | 50,626     | 183,636 | 234,262        | 4.3                    |
| 11PM - 12AM  | 47,559     | 53,199  | 100,758        | 1.85                   |
| 12AM - 1AM   | 35,339     | 33,179  | 68,518         | 1.26                   |
| 6AM - 7AM    | 28,770     | 99,470  | 128,240        | 2.35                   |
| 1AM - 2AM    | 22,919     | 19,760  | 42,679         | 0.78                   |
| 2AM - 3AM    | 13,762     | 11,387  | 25,149         | 0.46                   |
| 5AM - 6AM    | 10,830     | 32,251  | 43,081         | 0.79                   |
| 3AM - 4AM    | 7,603      | 7,486   | 15,089         | 0.28                   |
| 4AM - 5AM    | 5,632      | 8,285   | 13,917         | 0.26                   |

### Ride Duration Analysis by User Type

```sql
SELECT
 member_casual,
 sum(avg_ride_time) as total_avg_time,
 sum(electric_bike_avg_time) as electric_avg_time,
 sum(classic_avg_time) as calssic_avg_time,
 sum(docked_bike) as docked_avg_time,
 Round(sum(avg_ride_time) / (sum(sum(avg_ride_time)) OVER())*100,2) as avg_ride_time_percentage
FROM
(
SELECT
 member_casual,
 ROUND(AVG(ride_time),2) AS avg_ride_time,
 ROUND(AVG(CASE WHEN rideable_type = 'electric_bike' THEN ride_time END),2) AS electric_bike_avg_time,
 ROUND(AVG(CASE WHEN rideable_type = 'classic_bike' THEN ride_time END),2) AS classic_avg_time,
 ROUND(AVG(CASE WHEN rideable_type = 'docked_bike' THEN ride_time END),2) AS docked_bike
FROM `Cyclistic_ride_data_Feb23_to_Jan24.station_data`
GROUP BY
  member_casual
)
GROUP BY
 member_casual;
```

### Query Result:
| member_casual | total_avg_time | electric_avg_time | classic_avg_time | docked_avg_time | avg_ride_time_percentage |
|---------------|----------------|-------------------|------------------|-----------------|-------------------------|
| member        | 12.27          | 10.62             | 13.15            | N/A             | 34.62                   |
| casual        | 23.17          | 14.84             | 25.92            | 54.31           | 65.38                   |
