## Question3 (Different with the given choices):
```sql
SELECT
    COUNT(CASE WHEN trip_distance <= 1 THEN 1 END) AS "Up to 1 mile",
    COUNT(CASE WHEN trip_distance > 1 AND trip_distance <= 3 THEN 1 END) AS "Between 1 and 3 miles",
    COUNT(CASE WHEN trip_distance > 3 AND trip_distance <= 7 THEN 1 END) AS "Between 3 and 7 miles",
    COUNT(CASE WHEN trip_distance > 7 AND trip_distance <= 10 THEN 1 END) AS "Between 7 and 10 miles",
    COUNT(CASE WHEN trip_distance > 10 THEN 1 END) AS "Over 10 miles"
FROM public.green_tripdata
WHERE lpep_pickup_datetime >= '2019-10-01 00:00:00'
  AND lpep_pickup_datetime < '2019-11-01 00:00:00';
```
```sql
Up to 1 mile | Between 1 and 3 miles | Between 3 and 7 miles | Between 7 and 10 miles | Over 10 miles

78992 |   150921 |   90059 |   24082 |   32306
```

## Question4:
```sql
SELECT
    DATE(lpep_pickup_datetime) AS pickup_date,
    MAX(trip_distance) AS max_trip_distance
FROM public.green_tripdata
GROUP BY pickup_date
ORDER BY max_trip_distance DESC
LIMIT 1;
```
```sql
 pickup_date | max_trip_distance
-------------+-------------------
 2019-10-31  |            515.89
```
## Question5:
```sql
SELECT 
    "PULocationID",
    SUM("total_amount") AS total_amount_sum
FROM 
    public.green_tripdata
WHERE 
    "lpep_pickup_datetime" >= '2019-10-18 00:00:00'
    AND "lpep_pickup_datetime" < '2019-10-19 00:00:00'
GROUP BY 
    "PULocationID"
HAVING 
    SUM("total_amount") > 13000
ORDER BY 
    total_amount_sum DESC;
```
```sql
 PULocationID |  total_amount_sum
--------------+--------------------
           74 |  18686.67999999975
           75 |  16797.25999999984
          166 | 13029.789999999906
```
#### Based on these three PULocationID, I checked the zones from zone table.
## Question6:
```sql
SELECT 
    t2."Zone" AS dropoff_zone,
    MAX(g."tip_amount") AS max_tip
FROM 
    public.green_tripdata g
JOIN 
    public.taxi_zone_lookup t1 ON g."PULocationID" = t1."LocationID"
JOIN 
    public.taxi_zone_lookup t2 ON g."DOLocationID" = t2."LocationID"
WHERE 
    t1."Zone" = 'East Harlem North'
    AND g."lpep_pickup_datetime" >= '2019-10-01 00:00:00'
    AND g."lpep_pickup_datetime" < '2019-11-01 00:00:00'
GROUP BY 
    t2."Zone"
ORDER BY 
    max_tip DESC
LIMIT 1;
```
```sql
 dropoff_zone | max_tip
--------------+---------
 JFK Airport  |    87.3
```
