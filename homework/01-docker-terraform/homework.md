data-engineering-zoomcamp-2026

Homework 1: Docker, SQL and Terraform for Data Engineering Zoomcamp 2026


Question 1. Understanding Docker images

docker run -it \
    --rm \
    -v $(pwd)/test:/app/test \
    --entrypoint=bash \
    python:3.13


pip -V 
pip 25.3 from /usr/local/lib/python3.13/site-packages/pip (python 3.13)


Question 2. Understanding Docker networking and docker-compose

db:5432

Prepare the data


!wget https://github.com/DataTalksClub/nyc-tlc-data/releases/download/misc/taxi_zone_lookup.csv

import pandas as pd

df_zones = pd.read_csv ('taxi_zone_lookup.csv')

df_zones.to_sql(name='zones', con=engine, if_exists='replace')

df_zones.head()


!wget https://d37ci6vzurychx.cloudfront.net/trip-data/green_tripdata_2025-11.parquet

df_zones = pd.read_parquet ('green_tripdata_2025-11.parquet')

df_zones.to_sql(name='green_taxi_trips', con=engine, if_exists='replace')

df.head()
Question 3. Counting short trips

SELECT COUNT(*) AS trips_leq_1_mile
 FROM green_taxi_trips
 WHERE trip_distance <= 1
   AND lpep_pickup_datetime >= '2025-11-01'
   AND lpep_pickup_datetime < '2025-12-01';



+------------------+
| trips_leq_1_mile |
|------------------|
| 8007             |
+------------------+




Question 4. Longest trip for each day

SELECT
     DATE(lpep_pickup_datetime) AS pickup_day,
     MAX(trip_distance) AS longest_trip_distance
 FROM green_taxi_trips
 WHERE trip_distance < 100
 GROUP BY pickup_day
 ORDER BY longest_trip_distance DESC
 LIMIT 1;
+------------+-----------------------+
| pickup_day | longest_trip_distance |
|------------+-----------------------|
| 2025-11-14 | 88.03                 |
+------------+-----------------------+



Question 5. Biggest pickup zone

SELECT
     z."Zone" AS pickup_zone,
     SUM(t.total_amount) AS total_amount_sum
 FROM green_taxi_trips t
 JOIN zones z
     ON t."PULocationID" = z."LocationID"
 WHERE t.lpep_pickup_datetime >= '2025-11-18'
   AND t.lpep_pickup_datetime < '2025-11-19'
 GROUP BY z."Zone"
 ORDER BY total_amount_sum DESC
 LIMIT 1;
+-------------------+-------------------+
| pickup_zone       | total_amount_sum  |
|-------------------+-------------------|
| East Harlem North | 9281.919999999996 |
+-------------------+-------------------+


Question 6. Largest tip

SELECT
     dz."Zone" AS dropoff_zone,
     MAX(t.tip_amount) AS largest_tip
 FROM green_taxi_trips t
 JOIN zones pz
     ON t."PULocationID" = pz."LocationID"
 JOIN zones dz
     ON t."DOLocationID" = dz."LocationID"
 WHERE pz."Zone" = 'East Harlem North'
   AND t.lpep_pickup_datetime >= '2025-11-01'
   AND t.lpep_pickup_datetime < '2025-12-01'
 GROUP BY dz."Zone"
 ORDER BY largest_tip DESC
 LIMIT 1;
+----------------+-------------+
| dropoff_zone   | largest_tip |
|----------------+-------------|
| Yorkville West | 81.89       |
+----------------+-------------+

Question 7. Terraform Workflow

I'm guessing

terraform init, terraform apply -auto-approve, terraform destroy




Homework 2: Workflow Orchestration


Question 1. Within the execution for Yellow Taxi data for the year 2020 and month 12: what is the uncompressed file size (i.e. the output file yellow_tripdata_2020-12.csv of the extract task)?



Question 2. What is the rendered value of the variable file when the inputs taxi is set to green, year is set to 2020, and month is set to 04 during execution? 


Question 3. How many rows are there for the Yellow Taxi data for all CSV files in the year 2020? 


Question 4. How many rows are there for the Green Taxi data for all CSV files in the year 2020?

Question 5. How many rows are there for the Yellow Taxi data for the March 2021 CSV file? 

Question 6. How would you configure the timezone to New York in a Schedule trigger? 


