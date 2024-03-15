# Cyclistic : Rider Trend Analysis

## Company Profile

Cyclistic is a bike-share program with over 5,800 bicycles and 600 docking stations, known for its inclusivity with options like reclining bikes and hand tricycles. Serving people with disabilities and those preferring non-standard bikes, it balances leisure and commuting needs. Approximately 30% of riders use the bikes for daily work commutes, showcasing its practicality beyond recreational use.

## Objective

The objective is to analyze Cyclistic's historical bike trip data to differentiate between annual members and casual riders, understand the reasons why casual riders might opt for a membership, and assess the potential influence of digital media on marketing tactics. Through this analysis, I aim to design effective marketing strategies aimed at converting casual riders into annual members, ultimately increasing membership uptake for Cyclistic.

## Data Source
The data utilized for this project originates from Motivate LLC, a real-world company, under the specified license agreement [here](https://divvybikes.com/data-license-agreement). I've amalgamated 12 months of data (from February 2023 to January 2024) from this source into a unified dataset for comprehensive analysis.

## Data Cleanup

Initially, I consolidated data from 12 months into a single dataset, totaling 54,50,376 rows, each representing ride information. A meticulous review revealed missing 'start_station_names' and 'end_station_names' entries. For ride detail analysis, the complete dataset was utilized, while analysis involving station names excluded entries with null values, resulting in 41,30,450 rows.
