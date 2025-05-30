# Goodcab-analysis-project

🚖 Goodcabs - Unlocking Data-Driven Growth! 


📌 Overview
This project analyzes and provides strategic insights for Goodcabs, a rapidly growing cab service in India’s tier-2 cities. By leveraging data, I explored key business metrics to enhance growth and improve passenger satisfaction.

Goodcabs, operating in 10 tier-2 cities, supports local drivers and aims to offer better experiences to passengers. With ambitious performance goals for 2024, the company needed immediate insights to drive data-driven decision-making and boost performance.


📑 **Project Presentation:** [Here](https://data-visual-story.my.canva.site/)



📜 Problem Statement
Goodcabs’ Chief of Operations needed a comprehensive analysis of key performance metrics for trip volume, passenger satisfaction, repeat passenger rate, trip distribution, and more. With the task of analyzing and reporting this critical data, the goal was to provide actionable insights that would help Goodcabs hit its 2024 growth targets and improve passenger experience.



🎯 Goal & Purpose

🔹 Identify trends in trips, passenger behavior, and fare patterns.
Analyzed trip volume, passenger segmentation (new vs. repeat), and fare distribution to uncover key insights.

🔹 Assess city-wise performance to highlight growth opportunities.
Determined top-performing and underperforming cities based on trip volume, revenue, and passenger satisfaction.

🔹 Establish correlations between trip distance, fare, and passenger retention.
Examined fare efficiency, repeat passenger rates, and city-wise trends to understand their impact on business growth.

🔹 Support data-driven decision-making with structured SQL insights.
Provided actionable recommendations by extracting insights using SQL queries to answer business-critical questions.



⚙️ Approach & Tools

🔹 Data Cleaning & Exploration
Processed raw data to ensure consistency and accuracy before analysis.

🔹 SQL-Based Analysis
Executed queries to evaluate trip demand, repeat passenger behavior, peak-demand months, and fare distribution.

🔹 Insight Extraction & Recommendations
Derived key business insights to optimize performance, enhance customer satisfaction, and improve revenue strategies.



Primary Analysis:
Identify the top 3 and bottom 3 cities by total trips over the entire analysis period.

![Top Product](https://github.com/user-attachments/assets/29dd1d51-8418-4f27-83b8-945e0f1d59e6)
![Bottom product](https://github.com/user-attachments/assets/6dcb8aae-9cb1-489f-af16-e975cacbb3cd)

Calculate the average fare per trip for each city and compare it with the city's average trip distance. Identify the cities with the highest and lowest average fare per trip to assess pricing efficiency across locations.

![Average Fare](https://github.com/user-attachments/assets/08daa9b5-cd4b-4282-b664-ea305e95144d)

Calculate the average passenger and driver ratings for each city, segmented by passenger type (new vs. repeat). Identify cities with the highest and lowest average ratings.

![Average Rating](https://github.com/user-attachments/assets/a2516262-3b97-4f21-8908-725867e3fc78)

![Average Rating](https://github.com/user-attachments/assets/e6b18598-bc47-45f5-a270-f50da845bc5c)


For each city, identify the month with the highest total trips (peak demand) and the month with the lowest total trips (low demand). This analysis will help Goodcabs understand seasonal patterns and adjust resources accordingly.

![Demands](https://github.com/user-attachments/assets/05c89b15-d316-40b8-99de-ec9679b0d242)

Compare the total trips taken on weekdays versus weekends for each city over the six-month period. Identify cities with a strong preference for either weekend or weekday trips to understand demand variations.

![Day type](https://github.com/user-attachments/assets/2d880c4f-3faa-47f6-bd74-b0fc87939a18)

Analyse the frequency of trips taken by repeat passengers in each city (e.g., % of repeat passengers taking 2 trips, 3 trips, etc.). Identify which cities contribute most to higher trip frequencies among repeat passengers, and examine if there are distinguishable patterns between tourism-focused and business-focused cities.

![Repeat Passenger](https://github.com/user-attachments/assets/170cb91d-85ff-4ebc-9874-47bfa3cbcddf)


Analyse the Repeat Passenger Rate (RPR%) for each city across the six-month period. Identify the top 2 and bottom 2 cities based on their RPR% to determine which locations have the strongest and weakest rates.

Similarly, analyse the RPR% by month across all cities and identify the months with the highest and lowest repeat passenger rates. This will help to pinpoint any seasonal patterns or months with higher repeat passenger loyalty.

![Repeat Passenger %](https://github.com/user-attachments/assets/7d2f870b-ba3d-4b7f-a956-00d664560356)


![Repeat Passenger %](https://github.com/user-attachments/assets/5c01fe81-d5ad-4c51-afdc-4e3a18223038)


![Repeat Passenger %](https://github.com/user-attachments/assets/1ea7c040-833a-407b-b8be-287d649c767e)


📌 Ad hoc requests:

Q1 Generate a report that displays the total trips, average fare per km, average fare per trip, and the percentage contribution of each city’s trips to the overall trips. This report will help in assessing trip volume, pricing efficiency, and each city’s contribution to the overall trip count.

Query

```
 with cte as (
  Select
    city_name,
    count(distinct trip_id) as total_trips,
    avg(fare_amount / distance_travelled_km) as average_fare_per_km,
    sum(fare_amount) / count(distinct trip_id) as average_fare_per_trip,
    (count(distinct trip_id) / (select count(distinct trip_id) from fact_trips)) * 100 as contribution_to_total_trips
  from dim_city c
  join fact_trips f
    on c.city_id = f.city_id
  group by city_name
)
select
  city_name,
  total_trips,
  round(average_fare_per_km, 2) as average_fare_per_km,
  round(average_fare_per_trip, 2) as average_fare_per_trip,
  round(contribution_to_total_trips, 2) as contribution_to_total_trips
from cte;

```




Q2 Generate a report that evaluates the target performance for trips at the monthly and city level. For each city and month, compare the actual total trips with the target trips and categorize the performance as follows:

-- If actual trips are greater than target trips, mark it as "Above Target". -- If actual trips are less than or equal to target trips, mark it as "Below Target".

-- Additionally, calculate the % difference between actual and target trips to quantify the performance gap.

Query

```

 SELECT
    c.city_name,
    date_format(trp.date, '%M') AS month_name,
    COUNT(trip_id) AS actual_trips,
    mt.total_target_trips AS target_trips,
    CASE
        WHEN COUNT(trip_id) > mt.total_target_trips THEN "Above Target"
        WHEN COUNT(trip_id) <= mt.total_target_trips THEN "Below Target"
    END AS performance_status,
    CONCAT(ROUND(((COUNT(trip_id) - mt.total_target_trips) * 100 / COUNT(trip_id)), 2), '%') AS pct_difference
FROM fact_trips trp
JOIN dim_city c ON trp.city_id = c.city_id
JOIN monthly_target_trips AS mt ON trp.city_id = mt.city_id AND date_format(trp.date, '%M') = date_format(mt.month, '%M')
GROUP BY trp.city_id, date_format(trp.date, '%M'), mt.total_target_trips
ORDER BY month_name;

```

Q3 Generate a report that shows the percentage distribution of repeat passengers by the number of trips they have taken in each city. Calculate the percentage of repeat passengers who took 2 trips, 3 trips, and so on, up to 10 trips.

-- Each column should represent a trip count category, displaying the percentage of repeat passengers who fall into that category out of the total repeat passengers for that city.

Query

```

 WITH city_trip_data AS (
    SELECT 
        drt.city_id,
        c.city_name, 
        drt.trip_count, 
        drt.repeat_passenger_count,
        SUM(drt.repeat_passenger_count) OVER (PARTITION BY drt.city_id) AS total_repeat_passenger_count
    FROM dim_repeat_trip_distribution drt
    INNER JOIN dim_city c ON c.city_id = drt.city_id
)
SELECT 
    city_name,
    ROUND((SUM(CASE WHEN trip_count = 2 THEN repeat_passenger_count ELSE 0 END) / MAX(total_repeat_passenger_count)) * 100, 2) AS "2_trips_%",
    ROUND((SUM(CASE WHEN trip_count = 3 THEN repeat_passenger_count ELSE 0 END) / MAX(total_repeat_passenger_count)) * 100, 2) AS "3_trips_%",
    ROUND((SUM(CASE WHEN trip_count = 4 THEN repeat_passenger_count ELSE 0 END) / MAX(total_repeat_passenger_count)) * 100, 2) AS "4_trips_%",
    ROUND((SUM(CASE WHEN trip_count = 5 THEN repeat_passenger_count ELSE 0 END) / MAX(total_repeat_passenger_count)) * 100, 2) AS "5_trips_%",
    ROUND((SUM(CASE WHEN trip_count = 6 THEN repeat_passenger_count ELSE 0 END) / MAX(total_repeat_passenger_count)) * 100, 2) AS "6_trips_%",
    ROUND((SUM(CASE WHEN trip_count = 7 THEN repeat_passenger_count ELSE 0 END) / MAX(total_repeat_passenger_count)) * 100, 2) AS "7_trips_%",
    ROUND((SUM(CASE WHEN trip_count = 8 THEN repeat_passenger_count ELSE 0 END) / MAX(total_repeat_passenger_count)) * 100, 2) AS "8_trips_%",
    ROUND((SUM(CASE WHEN trip_count = 9 THEN repeat_passenger_count ELSE 0 END) / MAX(total_repeat_passenger_count)) * 100, 2) AS "9_trips_%",
    ROUND((SUM(CASE WHEN trip_count = 10 THEN repeat_passenger_count ELSE 0 END) / MAX(total_repeat_passenger_count)) * 100, 2) AS "10_trips_%"
FROM city_trip_data
GROUP BY city_id, city_name;

```

Q4 Generate a report that calculates the total new passengers for each city and ranks them based on this value. Identify the top 3 cities with the highest number of new passengers as well as the bottom 3 cities with the lowest number of new passengers, categorizing them as "Top 3" or "Bottom 3" accordingly.

Query

```

    WITH RankedCities AS
 (
    SELECT 
        c.city_name, 
        SUM(fps.new_passengers) AS Passenger,
        CASE 
            WHEN RANK() OVER (ORDER BY SUM(fps.new_passengers) DESC) <= 3 THEN 'Top 3'
            WHEN RANK() OVER (ORDER BY SUM(fps.new_passengers) ASC) <= 3 THEN 'Bottom 3'
            ELSE NULL
        END AS City_category
    FROM 
        fact_passenger_summary fps
    INNER JOIN 
        dim_city c ON c.city_id = fps.city_id
    GROUP BY 
        c.city_name
)
SELECT 
    city_name, 
    Passenger, 
    City_category
FROM 
    RankedCities
WHERE 
    City_category IS NOT NULL
ORDER BY 
    City_category, Passenger DESC;

```

Q5 Generate a report that identifies the month with the highest revenue for each city. For each city, display the month_name, the revenue amount for that month, and the percentage contribution of that month’s revenue to the city’s total revenue.

Query

```

 SELECT 
    city_name, 
    Higest_revenue_month, 
    Revenue, 
    Pecentage_contribution AS 'Pecentage_contribution (%)'
FROM (
    SELECT 
        c.city_name, 
        MONTHNAME(f.date) AS 'Higest_revenue_month', 
        SUM(f.fare_amount) AS 'Revenue', 
        ROUND((SUM(f.fare_amount) / (SELECT SUM(fare_amount) FROM fact_trips)) * 100, 2) AS 'Pecentage_contribution',
        ROW_NUMBER() OVER (PARTITION BY c.city_name ORDER BY SUM(f.fare_amount) DESC) AS revenue_rank
    FROM 
        fact_trips f
    INNER JOIN 
        dim_city c ON c.city_id = f.city_id
    GROUP BY 
        c.city_name, MONTHNAME(f.date)
) ranked_revenue
WHERE revenue_rank = 1;


```


Q6 Generate a report that calculates two metrics:

-- Monthly Repeat Passenger Rate: Calculate the repeat passenger rate for each city and month by comparing the number of repeat passengers to the total passengers.

-- City-wide Repeat Passenger Rate: Calculate the overall repeat passenger rate for each city, considering all passengers across months.

Query

```

         SELECT 
    city.city_name, 
    MONTHNAME(fp.month) AS 'month_name', 
    fp.total_passengers AS 'total_passengers', 
    fp.repeat_passengers AS 'repeat_passengers',  
    ROUND((fp.repeat_passengers / fp.total_passengers) * 100, 2) AS 'monthly_repeat_pax_rate', 
    city_rates.city_repeat_pax_rate
FROM 
    fact_passenger_summary fp
INNER JOIN dim_city city ON city.city_id = fp.city_id
INNER JOIN (
    SELECT 
        city_id, 
        SUM(total_passengers) AS total_passengers_sum, 
        SUM(repeat_passengers) AS repeat_passengers_sum, 
        ROUND((SUM(repeat_passengers) / SUM(total_passengers)) * 100, 2) AS city_repeat_pax_rate
    FROM 
        fact_passenger_summary
    GROUP BY 
        city_id
) city_rates ON city_rates.city_id = fp.city_id;

```
