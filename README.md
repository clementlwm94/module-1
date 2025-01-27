# module-1
# Question 1
docker run -it --entrypoint bash python:3.12.8
pip --version

# Question 2
use eye

# Question 3
SELECT 
    SUM(CASE 
        WHEN trip_distance <= 1 THEN 1
        ELSE 0 
    END) AS up_to_1_mile,
    
    SUM(CASE 
        WHEN trip_distance > 1 AND trip_distance <= 3 THEN 1
        ELSE 0 
    END) AS between_1_and_3_miles,
    
    SUM(CASE 
        WHEN trip_distance > 3 AND trip_distance <= 7 THEN 1
        ELSE 0 
    END) AS between_3_and_7_miles,
    
    SUM(CASE 
        WHEN trip_distance > 7 AND trip_distance <= 10 THEN 1
        ELSE 0 
    END) AS between_7_and_10_miles,
    
    SUM(CASE 
        WHEN trip_distance > 10 THEN 1
        ELSE 0 
    END) AS over_10_miles
FROM green_taxi_data
WHERE CAST(lpep_dropoff_datetime AS DATE) >= '2019-10-01' 
  AND CAST(lpep_dropoff_datetime AS DATE) < '2019-11-01';


# Question 4
SELECT 
    CAST(lpep_pickup_datetime AS DATE) AS "date",
    MAX(trip_distance) AS "longest_distance"
FROM 
    green_taxi_data
WHERE
    CAST(lpep_pickup_datetime AS DATE) = '2019-10-18'
GROUP BY 
    CAST(lpep_pickup_datetime AS DATE);

# Question 5
SELECT 
    "PULocationID",
    z."Zone" AS "zone1",
    z2."Zone" AS "zone2",
    SUM(total_amount) AS "total_amount2"
FROM 
    green_taxi_data t 
LEFT JOIN zone_data z
    ON t."PULocationID" = z."LocationID"
LEFT JOIN zone_data z2
    ON t."DOLocationID" = z2."LocationID"
WHERE
    CAST(lpep_pickup_datetime AS DATE) = '2019-10-18'
GROUP BY
    "PULocationID", 
    z."Zone",
    z2."Zone"
ORDER BY
    total_amount2 DESC;

# Question 6
SELECT 
    z2."Zone" AS "dropoff_zone",
    MAX(t."tip_amount") AS "max_tip"
FROM 
    green_taxi_data t
LEFT JOIN zone_data z
    ON t."PULocationID" = z."LocationID"
LEFT JOIN zone_data z2
    ON t."DOLocationID" = z2."LocationID"
WHERE
    z."Zone" = 'East Harlem North' AND
    CAST(lpep_pickup_datetime AS DATE) >= '2019-10-01' AND
    CAST(lpep_pickup_datetime AS DATE) < '2019-11-01'
GROUP BY
    z2."Zone"
ORDER BY
    "max_tip" DESC 

