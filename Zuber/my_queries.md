### Exploratory data analysis  

**1.** Print the company_name field. Find the number of taxi rides for each taxi company for November 15-16, 2017, name the resulting field trips_amount and print it, too. Sort the results by the trips_amount field in descending order.
```sql  
SELECT
    company_name,
    COUNT(trips.trip_id) AS trips_amount
FROM 
    cabs
LEFT JOIN trips ON trips.cab_id = cabs.cab_id
WHERE 
    CAST(start_ts AS date) BETWEEN '2017-11-15' AND '2017-11-16'
GROUP BY 
    company_name
ORDER BY
    COUNT(trips.trip_id) DESC;
````  
<details>
  <summary>Results</summary>

| Company Name                                 | Trips Amount |
|----------------------------------------------|--------------|
| Flash Cab                                    | 19558        |
| Taxi Affiliation Services                    | 11422        |
| Medallion Leasin                             | 10367        |
| Yellow Cab                                   | 9888         |
| Taxi Affiliation Service Yellow              | 9299         |
| Chicago Carriage Cab Corp                    | 9181         |
| City Service                                 | 8448         |
| Sun Taxi                                     | 7701         |
| Star North Management LLC                    | 7455         |
| Blue Ribbon Taxi Association Inc.             | 5953         |
| Choice Taxi Association                      | 5015         |
| Globe Taxi                                   | 4383         |
| Dispatch Taxi Affiliation                    | 3355         |
| Nova Taxi Affiliation Llc                    | 3175         |
| Patriot Taxi Dba Peace Taxi Associat         | 2235         |
| Checker Taxi Affiliation                      | 2216         |
| Blue Diamond                                 | 2070         |
| Chicago Medallion Management                 | 1955         |
| 24 Seven Taxi                                | 1775         |
| Chicago Medallion Leasing INC                | 1607         |
| Checker Taxi                                 | 1486         |
| American United                              | 1404         |
| Chicago Independents                         | 1296         |
| KOAM Taxi Association                        | 1259         |
| Chicago Taxicab                              | 1014         |
| Top Cab Affiliation                          | 978          |
| Gold Coast Taxi                              | 428          |
| Service Taxi Association                      | 402          |
| 5 Star Taxi                                  | 310          |
| 303 Taxi                                     | 250          |
| Setare Inc                                   | 230          |
| American United Taxi Affiliation             | 210          |
| Leonard Cab Co                               | 147          |
| Metro Jet Taxi A                             | 146          |
| Norshore Cab                                 | 127          |
| 6742 - 83735 Tasha ride inc                  | 39           |
| 3591 - 63480 Chuks Cab                       | 37           |
| 1469 - 64126 Omar Jada                       | 36           |
| 0118 - 42111 Godfrey S.Awir                  | 33           |
| 6743 - 78771 Luhak Corp                      | 33           |
| 6574 - Babylon Express Inc.                  | 31           |
| Chicago Star Taxicab                         | 29           |
| 1085 - 72312 N and W Cab Co                  | 29           |
| 2809 - 95474 C & D Cab Co Inc.               | 29           |
| 2092 - 61288 Sbeih company                   | 27           |
| 3011 - 66308 JBL Cab Inc.                    | 25           |
| 3620 - 52292 David K. Cab Corp.              | 21           |
| 4615 - 83503 Tyrone Henderson                | 21           |
| 3623 - 72222 Arrington Enterprises           | 20           |
| 5074 - 54002 Ahzmi Inc                       | 16           |
| 2823 - 73307 Lee Express Inc                 | 15           |
| 4623 - 27290 Jay Kim                         | 15           |
| 3721 - Santamaria Express, Alvaro Santamaria | 14           |
| 5006 - 39261 Salifu Bawa                     | 14           |
| 2192 - 73487 Zeymane Corp                    | 14           |
| 6057 - 24657 Richard Addo                    | 13           |
| 5997 - 65283 AW Services Inc.                | 12           |
| Metro Group                                  | 11           |
| 5062 - 34841 Sam Mestas                      | 8            |
| 4053 - 40193 Adwar H. Nikola                 | 7            |
| 2733 - 74600 Benny Jona                      | 7            |
| 5874 - 73628 Sergey Cab Corp.                 | 5            |
| 2241 - 44667 - Felman Corp, Manuel Alonso    | 3            |
| 3556 - 36214 RC Andrews Cab                  | 2            |
</details>  

**2.** Find the number of rides for every taxi companies whose name contains the words "Yellow" or "Blue" for November 1-7, 2017. Name the resulting variable trips_amount. Group the results by the company_name field.
```sql
SELECT
    company_name,
    COUNT(trip_id) AS trips_amount
FROM trips
LEFT JOIN cabs ON cabs.cab_id = trips.cab_id
WHERE
    (company_name LIKE '%Yellow%' 
    OR company_name LIKE '%Blue%')
    AND CAST(start_ts AS date) BETWEEN '2017-11-01' AND '2017-11-07'
GROUP BY company_name;
````
**Results:**
  
  | company_name |	trips_amount |
  | -------------| --------------|
  | Blue Diamond |	6764 |
  | Blue Ribbon Taxi Association Inc.	| 17675 |
  | Taxi Affiliation Service Yellow	| 29213 |
  | Yellow Cab |	33668 |

**3.** For November 1-7, 2017, the most popular taxi companies were Flash Cab and Taxi Affiliation Services. Find the number of rides for these two companies and name the resulting variable trips_amount. Join the rides for all other companies in the group "Other." Group the data by taxi company names. Name the field with taxi company names company. Sort the result in descending order by trips_amount.
```sql
SELECT
    CASE WHEN company_name = 'Flash Cab' THEN 'Flash Cab'
    WHEN company_name = 'Taxi Affiliation Services' THEN 'Taxi Affiliation Services'
    ELSE 'Other' END AS company,
    COUNT(DISTINCT trip_id) AS trips_amount
FROM cabs
LEFT JOIN trips ON trips.cab_id = cabs.cab_id
WHERE
     CAST(start_ts AS date) BETWEEN '2017-11-01' AND '2017-11-07'
GROUP BY
    company
ORDER BY
    COUNT(trip_id) DESC;
````
**Results:**
  
  | company |	trips_amount |
  | -------------| --------------|
  | Other |	335771 |  
  | Flash Cab |	64084 |
  | Taxi Affiliation Services	| 37583 |

  
### Determine if and how the duration of rides from the Loop to O'Hare International Airport changes on rainy Saturdays compared to other days of the week and other weather conditions.

**4.** Retrieve the identifiers of the O'Hare and Loop neighborhoods from the neighborhoods table.
```sql
SELECT
    neighborhood_id,
    name
FROM 
    neighborhoods
WHERE 
    name LIKE 'O%Hare'
    OR name LIKE 'Loop';
````
**Results:**
  
  | neighborhood_id |	name |
  | -------------| --------------|
  | 50 |	Loop |
  | 63 |	O'Hare |

**5.** For each hour, retrieve the weather condition records from the weather_records table. Using the CASE operator, break all hours into two groups: Bad if the description field contains the words rain or storm, and Good for others. Name the resulting field weather_conditions. The final table must include two fields: date and hour (ts) and weather_conditions.
```sql
SELECT
    ts,
    CASE WHEN description LIKE '%rain%' 
    OR description LIKE '%storm%' THEN 'Bad'
    ELSE 'Good' END AS weather_conditions
FROM
    weather_records
````
<details>
  <summary>Results</summary>

| ts                   | weather_conditions |
|----------------------|--------------------|
| 2017-11-01 00:00:00 | Good               |
| 2017-11-01 01:00:00 | Good               |
| 2017-11-01 02:00:00 | Good               |
| 2017-11-01 03:00:00 | Good               |
| 2017-11-01 04:00:00 | Good               |
| 2017-11-01 05:00:00 | Good               |
| 2017-11-01 06:00:00 | Good               |
| 2017-11-01 07:00:00 | Good               |
| 2017-11-01 08:00:00 | Good               |
| 2017-11-01 09:00:00 | Good               |
| 2017-11-01 10:00:00 | Good               |
| 2017-11-01 11:00:00 | Good               |
| 2017-11-01 12:00:00 | Good               |
| 2017-11-01 13:00:00 | Good               |
| 2017-11-01 14:00:00 | Good               |
| 2017-11-01 15:00:00 | Good               |
| 2017-11-01 16:00:00 | Good               |
| 2017-11-01 17:00:00 | Good               |
| 2017-11-01 18:00:00 | Good               |
| 2017-11-01 19:00:00 | Good               |
| 2017-11-01 20:00:00 | Good               |
| 2017-11-01 21:00:00 | Good               |
| 2017-11-01 22:00:00 | Good               |
| 2017-11-01 23:00:00 | Good               |
| 2017-11-02 00:00:00 | Good               |
| 2017-11-02 01:00:00 | Good               |
| 2017-11-02 02:00:00 | Good               |
| 2017-11-02 03:00:00 | Bad                |
| 2017-11-02 04:00:00 | Bad                |
| 2017-11-02 05:00:00 | Bad                |
| 2017-11-02 06:00:00 | Bad                |
| 2017-11-02 07:00:00 | Bad                |
| 2017-11-02 08:00:00 | Good               |
| 2017-11-02 09:00:00 | Good               |
| 2017-11-02 10:00:00 | Good               |
| 2017-11-02 11:00:00 | Good               |
| 2017-11-02 12:00:00 | Bad                |
| 2017-11-02 13:00:00 | Good               |
| 2017-11-02 14:00:00 | Good               |
| 2017-11-02 15:00:00 | Good               |
| 2017-11-02 16:00:00 | Good               |
| 2017-11-02 17:00:00 | Good               |
| 2017-11-02 18:00:00 | Good               |
| 2017-11-02 19:00:00 | Good               |
| 2017-11-02 20:00:00 | Bad                |
| 2017-11-02 21:00:00 | Bad                |
| 2017-11-02 22:00:00 | Good               |
| 2017-11-02 23:00:00 | Good               |
| 2017-11-03 00:00:00 | Bad                |
| 2017-11-03 01:00:00 | Good               |
| 2017-11-03 02:00:00 | Good               |
| 2017-11-03 03:00:00 | Good               |
| 2017-11-03 04:00:00 | Good               |
| 2017-11-03 05:00:00 | Good               |
| 2017-11-03 06:00:00 | Good               |
| 2017-11-03 07:00:00 | Good               |
| 2017-11-03 08:00:00 | Good               |
| 2017-11-03 09:00:00 | Good               |
| 2017-11-03 10:00:00 | Good               |
| 2017-11-03 11:00:00 | Good               |
| 2017-11-03 12:00:00 | Good               |
| 2017-11-03 13:00:00 | Good               |
| 2017-11-03 14:00:00 | Good               |
| 2017-11-03 15:00:00 | Good               |
| 2017-11-03 16:00:00 | Good               |
| 2017-11-03 17:00:00 | Good               |
| 2017-11-03 18:00:00 | Good               |
| 2017-11-03 19:00:00 | Good               |
| 2017-11-03 20:00:00 | Good               |
| 2017-11-03 21:00:00 | Good               |
| 2017-11-03 22:00:00 | Good               |
| 2017-11-03 23:00:00 | Good               |
| 2017-11-04 00:00:00 | Good               |
| 2017-11-04 01:00:00 | Good               |
| 2017-11-04 02:00:00 | Good               |
| 2017-11-04 03:00:00 | Good               |
| 2017-11-04 04:00:00 | Good               |
| 2017-11-04 05:00:00 | Good               |
| 2017-11-04 06:00:00 | Good               |
| 2017-11-04 07:00:00 | Good               |
| 2017-11-04 08:00:00 | Good               |
| 2017-11-04 09:00:00 | Good               |
| 2017-11-04 10:00:00 | Good               |
| 2017-11-04 11:00:00 | Good               |
| 2017-11-04 12:00:00 | Good               |
| 2017-11-04 13:00:00 | Good               |
| 2017-11-04 14:00:00 | Good               |
| 2017-11-04 15:00:00 | Good               |
| 2017-11-04 16:00:00 | Bad                |
| 2017-11-04 17:00:00 | Bad                |
| 2017-11-04 18:00:00 | Bad                |
| 2017-11-04 19:00:00 | Good               |
| 2017-11-04 20:00:00 | Good               |
| 2017-11-04 21:00:00 | Good               |
| 2017-11-04 22:00:00 | Good               |
| 2017-11-04 23:00:00 | Good               |
| 2017-11-05 00:00:00 | Good               |
| 2017-11-05 01:00:00 | Bad                |
| 2017-11-05 02:00:00 | Good               |
| 2017-11-05 03:00:00 | Good               |
| 2017-11-05 04:00:00 | Bad                |
| 2017-11-05 05:00:00 | Bad                |
| 2017-11-05 06:00:00 | Good               |
| 2017-11-05 07:00:00 | Good               |
| 2017-11-05 08:00:00 | Good               |
| 2017-11-05 09:00:00 | Good               |
| 2017-11-05 10:00:00 | Good               |
| 2017-11-05 11:00:00 | Good               |
| 2017-11-05 12:00:00 | Good               |
| 2017-11-05 13:00:00 | Good               |
| 2017-11-05 14:00:00 | Bad                |
| 2017-11-05 15:00:00 | Good               |
| 2017-11-05 16:00:00 | Bad                |
| 2017-11-05 17:00:00 | Good               |
| 2017-11-05 18:00:00 | Bad                |
| 2017-11-05 19:00:00 | Bad                |
| 2017-11-05 20:00:00 | Bad                |
| 2017-11-05 21:00:00 | Good               |
| 2017-11-05 22:00:00 | Good               |
| 2017-11-05 23:00:00 | Good               |
| 2017-11-06 00:00:00 | Good               |
| 2017-11-06 01:00:00 | Good               |
| 2017-11-06 02:00:00 | Good               |
| 2017-11-06 03:00:00 | Good               |
| 2017-11-06 04:00:00 | Good               |
| 2017-11-06 05:00:00 | Good               |
| 2017-11-06 06:00:00 | Good               |
| 2017-11-06 07:00:00 | Good               |
| 2017-11-06 08:00:00 | Good               |
| 2017-11-06 09:00:00 | Good               |
| 2017-11-06 10:00:00 | Good               |
| 2017-11-06 11:00:00 | Good               |
| 2017-11-06 12:00:00 | Good               |
| 2017-11-06 13:00:00 | Good               |
| 2017-11-06 14:00:00 | Good               |
| 2017-11-06 15:00:00 | Good               |
| 2017-11-06 16:00:00 | Good               |
| 2017-11-06 17:00:00 | Good               |
| 2017-11-06 18:00:00 | Good               |
| 2017-11-06 19:00:00 | Good               |
| 2017-11-06 20:00:00 | Good               |
| 2017-11-06 21:00:00 | Good               |
| 2017-11-06 22:00:00 | Good               |
| 2017-11-06 23:00:00 | Good               |
| 2017-11-07 00:00:00 | Good               |
| 2017-11-07 01:00:00 | Good               |
| 2017-11-07 02:00:00 | Good               |
| 2017-11-07 03:00:00 | Good               |
| 2017-11-07 04:00:00 | Good               |
| 2017-11-07 05:00:00 | Good               |
| 2017-11-07 06:00:00 | Good               |
| 2017-11-07 07:00:00 | Good               |
| 2017-11-07 08:00:00 | Good               |
| 2017-11-07 09:00:00 | Good               |
| 2017-11-07 10:00:00 | Good               |
| 2017-11-07 11:00:00 | Good               |
| 2017-11-07 12:00:00 | Good               |
| 2017-11-07 13:00:00 | Good               |
| 2017-11-07 14:00:00 | Good               |
| 2017-11-07 15:00:00 | Good               |
| 2017-11-07 16:00:00 | Good               |
| 2017-11-07 17:00:00 | Good               |
| 2017-11-07 18:00:00 | Good               |
| 2017-11-07 19:00:00 | Good               |
| 2017-11-07 20:00:00 | Good               |
| 2017-11-07 21:00:00 | Good               |
| 2017-11-07 22:00:00 | Good               |
| 2017-11-07 23:00:00 | Bad                |
| 2017-11-08 00:00:00 | Bad               
| 2017-11-08 01:00:00 | Good                |
| 2017-11-08 02:00:00 | Good                |
| 2017-11-08 03:00:00 | Good                |
| 2017-11-08 04:00:00 | Good                |
| 2017-11-08 05:00:00 | Good                |
| 2017-11-08 06:00:00 | Good                |
| 2017-11-08 07:00:00 | Good                |
| 2017-11-08 08:00:00 | Good                |
| 2017-11-08 09:00:00 | Good                |
| 2017-11-08 10:00:00 | Good                |
| 2017-11-08 11:00:00 | Good                |
| 2017-11-08 12:00:00 | Good                |
| 2017-11-08 13:00:00 | Good                |
| 2017-11-08 14:00:00 | Good                |
| 2017-11-08 15:00:00 | Good                |
| 2017-11-08 16:00:00 | Good                |
| 2017-11-08 17:00:00 | Good                |
| 2017-11-08 18:00:00 | Good                |
| 2017-11-08 19:00:00 | Good                |
| 2017-11-08 20:00:00 | Good                |
| 2017-11-08 21:00:00 | Good                |
| 2017-11-08 22:00:00 | Good                |
| 2017-11-08 23:00:00 | Good                |
| 2017-11-09 00:00:00 | Good                |
| 2017-11-09 01:00:00 | Good                |
| 2017-11-09 02:00:00 | Good                |
| 2017-11-09 03:00:00 | Good                |
| 2017-11-09 04:00:00 | Good                |
| 2017-11-09 05:00:00 | Good                |
| 2017-11-09 06:00:00 | Good                |
| 2017-11-09 07:00:00 | Good                |
</details>

**6.** Retrieve from the trips table all the rides that started in the Loop (pickup_location_id: 50) on a Saturday and ended at O'Hare (dropoff_location_id: 63). Get the weather conditions for each ride. Use the method you applied in the previous task. Also, retrieve the duration of each ride. Ignore rides for which data on weather conditions is not available.
```sql
SELECT
    trips.start_ts,
    CASE WHEN weather_records.description LIKE '%rain%' 
    OR weather_records.description LIKE '%storm%' THEN 'Bad'
    ELSE 'Good' END AS weather_conditions,
    trips.duration_seconds
FROM trips
INNER JOIN weather_records ON weather_records.ts = trips.start_ts
WHERE
    trips.pickup_location_id = 50
    AND trips.dropoff_location_id = 63
    AND EXTRACT(DOW FROM trips.start_ts) = 6
ORDER BY trips.trip_id;
````
<details>
  <summary>Results</summary> 

  | start_ts             | weather_conditions | duration_seconds |
|----------------------|---------------------|-------------------|
| 2017-11-25 12:00:00 | Good                | 1380             |
| 2017-11-25 16:00:00 | Good                | 2410             |
| 2017-11-25 14:00:00 | Good                | 1920             |
| 2017-11-25 12:00:00 | Good                | 1543             |
| 2017-11-04 10:00:00 | Good                | 2512             |
| 2017-11-11 07:00:00 | Good                | 1440             |
| 2017-11-11 04:00:00 | Good                | 1320             |
| 2017-11-04 16:00:00 | Bad                 | 2969             |
| 2017-11-18 11:00:00 | Good                | 2280             |
| 2017-11-04 16:00:00 | Bad                 | 3120             |
| 2017-11-11 15:00:00 | Good                | 4800             |
| 2017-11-04 05:00:00 | Good                | 1260             |
| 2017-11-11 06:00:00 | Good                | 1346             |
| 2017-11-04 04:00:00 | Good                | 1333             |
| 2017-11-04 11:00:00 | Good                | 2574             |
| 2017-11-11 12:00:00 | Good                | 2441             |
| 2017-11-04 14:00:00 | Good                | 3300             |
| 2017-11-11 14:00:00 | Good                | 2460             |
| 2017-11-11 12:00:00 | Good                | 2040             |
| 2017-11-18 06:00:00 | Good                | 1500             |
| 2017-11-04 11:00:00 | Good                | 2040             |
| 2017-11-11 08:00:00 | Good                | 1470             |
| 2017-11-04 08:00:00 | Good                | 1546             |
| 2017-11-11 16:00:00 | Good                | 2100             |
| 2017-11-25 13:00:00 | Good                | 60               |
| 2017-11-04 12:00:00 | Good                | 2640             |
| 2017-11-25 10:00:00 | Good                | 1502             |
| 2017-11-11 12:00:00 | Good                | 1915             |
| 2017-11-04 12:00:00 | Good                | 2769             |
| 2017-11-11 13:00:00 | Good                | 2250             |
| 2017-11-11 04:00:00 | Good                | 1260             |
| 2017-11-18 14:00:00 | Good                | 2826             |
| 2017-11-04 14:00:00 | Good                | 3360             |
| 2017-11-04 14:00:00 | Good                | 3180             |
| 2017-11-25 20:00:00 | Good                | 2100             |
| 2017-11-04 10:00:00 | Good                | 1800             |
| 2017-11-11 12:00:00 | Good                | 2289             |
| 2017-11-04 08:00:00 | Good                | 1494             |
| 2017-11-11 11:00:00 | Good                | 1560             |
| 2017-11-18 12:00:00 | Bad                 | 1980             |
| 2017-11-11 13:00:00 | Good                | 2115             |
| 2017-11-11 10:00:00 | Good                | 1506             |
| 2017-11-04 12:00:00 | Good                | 2580             |
| 2017-11-04 17:00:00 | Bad                 | 2460             |
| 2017-11-11 09:00:00 | Good                | 1620             |
| 2017-11-04 06:00:00 | Good                | 1163             |
| 2017-11-04 05:00:00 | Good                | 1533             |
| 2017-11-11 04:00:00 | Good                | 1477             |
| 2017-11-11 19:00:00 | Good                | 1984             |
| 2017-11-04 13:00:00 | Good                | 2940             |
| 2017-11-04 07:00:00 | Good                | 1320             |
| 2017-11-04 06:00:00 | Good                | 1440             |
| 2017-11-11 06:00:00 | Good                | 1260             |
| 2017-11-11 08:00:00 | Good                | 1560             |
| 2017-11-04 09:00:00 | Good                | 1683             |
| 2017-11-11 05:00:00 | Good                | 1343             |
| 2017-11-18 06:00:00 | Good                | 1742             |
| 2017-11-04 09:00:00 | Good                | 1560             |
| 2017-11-11 08:00:00 | Good                | 1358             |
| 2017-11-11 12:00:00 | Good                | 1980             |
| 2017-11-04 16:00:00 | Bad                 | 2760             |
| 2017-11-18 12:00:00 | Bad                 | 2460             |
| 2017-11-18 10:00:00 | Bad                 | 1440             |
| 2017-11-25 14:00:00 | Good                | 1620             |
| 2017-11-11 08:00:00 | Good                | 1415             |
| 2017-11-25 05:00:00 | Good                | 1325             |
| 2017-11-18 06:00:00 | Good                | 2100             |
| 2017-11-11 08:00:00 | Good                | 1320             |
| 2017-11-11 10:00:00 | Good                | 1260             |
| 2017-11-11 06:00:00 | Good                | 1260             |
| 2017-11-11 08:00:00 | Good                | 1200             |
| 2017-11-25 08:00:00 | Good                | 1320             |
| 2017-11-11 18:00:00 | Good                | 2280             |
| 2017-11-25 10:00:00 | Good                | 1320             |
| 2017-11-04 06:00:00 | Good                | 1140             |
| 2017-11-11 18:00:00 | Good                | 2520             |
| 2017-11-18 16:00:00 | Bad                 | 3000             |
| 2017-11-11 19:00:00 | Good                | 1920             |
| 2017-11-04 18:00:00 | Bad                 | 2363             |
| 2017-11-04 14:00:00 | Good                | 3084             |
| 2017-11-11 08:00:00 | Good                | 1380             |
| 2017-11-11 08:00:00 | Good                | 1380             |
| 2017-11-04 09:00:00 | Good                | 1380             |
| 2017-11-04 09:00:00 | Good                | 1380             |
| 2017-11-11 12:00:00 | Good                | 2213             |
| 2017-11-11 08:00:00 | Good                | 1140             |
| 2017-11-11 10:00:00 | Good                | 1435             |
| 2017-11-04 10:00:00 | Good                | 2460             |
| 2017-11-11 07:00:00 | Good                | 1200             |
| 2017-11-04 15:00:00 | Good                | 3201             |
| 2017-11-11 11:00:00 | Good                | 2074             |
| 2017-11-18 11:00:00 | Good                | 2843             |
| 2017-11-11 17:00:00 | Good                | 2426             |
| 2017-11-04 09:00:00 | Good                | 1740             |
| 2017-11-25 07:00:00 | Good                | 2340             |
| 2017-11-18 05:00:00 | Good                | 2075             |
| 2017-11-18 07:00:00 | Bad                 | 1511             |
| 2017-11-11 18:00:00 | Good                | 2220             |
| 2017-11-04 10:00:00 | Good                | 2551             |
| 2017-11-11 16:00:00 | Good                | 2062             |
| 2017-11-04 12:00:00 | Good                | 2999             |
| 2017-11-04 08:00:00 | Good                | 1677             |
| 2017-11-04 06:00:00 | Good                | 1177             |
| 2017-11-11 06:00:00 | Good                | 1475             |
| 2017-11-25 08:00:00 | Good                | 1277             |
| 2017-11-11 04:00:00 | Good                | 1213             |
| 2017-11-18 13:00:00 | Bad                 | 4044             |
| 2017-11-04 21:00:00 | Good                | 1680             |
| 2017-11-04 18:00:00 | Bad                 | 1980             |
| 2017-11-25 18:00:00 | Good                | 2760             |
| 2017-11-11 09:00:00 | Good                | 1380             |
| 2017-11-11 07:00:00 | Good                | 1380             |
| 2017-11-18 08:00:00 | Bad                 | 1320             |
| 2017-11-11 16:00:00 | Good                | 2591             |
| 2017-11-11 08:00:00 | Good                | 1260             |
| 2017-11-11 07:00:00 | Good                | 1380             |
| 2017-11-11 10:00:00 | Good                | 1440             |
| 2017-11-04 14:00:00 | Good                | 3240             |
| 2017-11-04 16:00:00 | Bad                 | 2782             |
| 2017-11-04 14:00:00 | Good                | 3120             |
| 2017-11-04 19:00:00 | Good                | 1869             |
| 2017-11-11 06:00:00 | Good                | 1218             |
| 2017-11-11 11:00:00 | Good                | 1620             |
| 2017-11-11 10:00:00 | Good                | 1380             |
| 2017-11-11 08:00:00 | Good                | 1380             |
| 2017-11-11 09:00:00 | Good                | 1380             |
| 2017-11-11 07:00:00 | Good                | 1320             |
| 2017-11-04 14:00:00 | Good                | 3300             |
| 2017-11-04 13:00:00 | Good                | 3060             |
| 2017-11-11 13:00:00 | Good                | 2100             |
| 2017-11-18 14:00:00 | Good                | 3540             |
| 2017-11-11 21:00:00 | Good                | 1920            |
| 2017-11-11 17:00:00   | Good                | 2160             |
| 2017-11-11 13:00:00   | Good                | 2123             |
| 2017-11-11 07:00:00   | Good                | 1384             |
| 2017-11-11 10:00:00   | Good                | 1260             |
| 2017-11-18 15:00:00   | Good                | 3480             |
| 2017-11-11 12:00:00   | Good                | 2071             |
| 2017-11-18 13:00:00   | Bad                 | 3300             |
| 2017-11-25 13:00:00   | Good                | 1560             |
| 2017-11-04 12:00:00   | Good                | 2760             |
| 2017-11-18 12:00:00   | Bad                 | 3024             |
| 2017-11-11 11:00:00   | Good                | 1380             |
| 2017-11-25 06:00:00   | Good                | 1200             |
| 2017-11-11 06:00:00   | Good                | 1667             |
| 2017-11-18 18:00:00   | Good                | 2056             |
| 2017-11-11 10:00:00   | Good                | 1473             |
| 2017-11-11 17:00:00   | Good                | 2460             |
| 2017-11-11 10:00:00   | Good                | 1740             |
| 2017-11-11 14:00:00   | Good                | 2340             |
| 2017-11-04 16:00:00   | Bad                 | 3180             |
| 2017-11-04 11:00:00   | Good                | 2220             |
| 2017-11-11 12:00:00   | Good                | 2240             |
| 2017-11-04 14:00:00   | Good                | 2778             |
| 2017-11-18 06:00:00   | Good                | 1420             |
| 2017-11-04 14:00:00   | Good                | 3480             |
| 2017-11-25 20:00:00   | Good                | 1980             |
| 2017-11-18 10:00:00   | Bad                 | 2055             |
| 2017-11-11 15:00:00   | Good                | 2380             |
| 2017-11-04 08:00:00   | Good                | 1539             |
| 2017-11-25 10:00:00   | Good                | 1591             |
| 2017-11-18 14:00:00   | Good                | 2588             |
| 2017-11-11 07:00:00   | Good                | 0                |
| 2017-11-04 22:00:00   | Good                | 1380             |
| 2017-11-04 12:00:00   | Good                | 2220             |
| 2017-11-04 08:00:00   | Good                | 1380             |
| 2017-11-04 14:00:00   | Good                | 2820             |
| 2017-11-11 09:00:00   | Good                | 0                |
| 2017-11-18 09:00:00   | Bad                 | 1260             |
| 2017-11-18 13:00:00   | Bad                 | 2940             |
| 2017-11-18 16:00:00   | Bad                 | 2340             |
| 2017-11-18 12:00:00   | Bad                 | 2220             |
| 2017-11-25 11:00:00   | Good                | 1140             |
| 2017-11-11 10:00:00   | Good                | 1239             |
| 2017-11-04 16:00:00   | Bad                 | 3130             |
| 2017-11-18 15:00:00   | Good                | 2877             |
| 2017-11-11 03:00:00   | Good                | 1162             |
| 2017-11-04 14:00:00   | Good                | 3060             |
| 2017-11-11 13:00:00   | Good                | 1680             |
| 2017-11-04 10:00:00   | Good                | 2112             |
| 2017-11-04 11:00:00   | Good                | 2328             |
| 2017-11-11 05:00:00   | Good                | 1504             |
| 2017-11-04 06:00:00   | Good                | 1439             |
| 2017-11-18 16:00:00   | Bad                 | 2811             |
| 2017-11-18 11:00:00   | Good                | 2094             |
| 2017-11-11 06:00:00   | Good                | 1430             |
| 2017-11-18 12:00:00   | Bad                 | 3026             |
| 2017-11-04 14:00:00   | Good                | 3120             |
| 2017-11-25 12:00:00   | Good                | 1380             |
| 2017-11-18 14:00:00   | Good                | 2994             |
| 2017-11-11 11:00:00   | Good                | 1620             |
| 2017-11-04 12:00:00   | Good                | 2640             |
| 2017-11-18 00:00:00   | Bad                 | 480              |
| 2017-11-18 19:00:00   | Good                | 0                |
| 2017-11-11 10:00:00   | Good                | 1414             |
| 2017-11-11 12:00:00   | Good                | 1981             |
| 2017-11-11 08:00:00   | Good                | 1200             |
| 2017-11-04 11:00:00   | Good                | 2160             |
| 2017-11-11 15:00:00   | Good                | 2400             |
| 2017-11-11 20:00:00   | Good                | 1500             |
  </details>
  
**7.** Compute the average duration for the different weather conditions
```sql
SELECT 
    weather_conditions, 
    AVG(duration_seconds) AS avg_duration_seconds
FROM
    (SELECT
        trips.start_ts,
        CASE WHEN weather_records.description LIKE '%rain%' 
        OR weather_records.description LIKE '%storm%' THEN 'Bad'
        ELSE 'Good' END AS weather_conditions,
        trips.duration_seconds
    FROM trips
    INNER JOIN weather_records ON weather_records.ts = trips.start_ts
    WHERE
        trips.pickup_location_id = 50
        AND trips.dropoff_location_id = 63
        AND EXTRACT(DOW FROM trips.start_ts) = 6
    ORDER BY trips.trip_id
    ) AS trip_details
   GROUP BY weather_conditions;
````
**Results:**
  
  | weather_conditions | avg_duration_seconds |
  | -------------| --------------|
  | Bad	| 2427.21 |
  | Good |	1999.68 |
