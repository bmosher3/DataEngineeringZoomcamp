1. 42084899
	SELECT 
	    EXTRACT(YEAR FROM pickup_datetime),
	    count(*) 
	FROM trips_data_all.fhv_tripdata
	GROUP BY EXTRACT(YEAR FROM pickup_datetime)

2. 792
	SELECT 
    	EXTRACT(YEAR FROM pickup_datetime),
    	count(DISTINCT dispatching_base_num) 
	FROM trips_data_all.fhv_tripdata
	GROUP BY EXTRACT(YEAR FROM pickup_datetime)

3. Partition by dropoff_datetime, and cluster by dispatching_base_num

4. Count = 26647
   Estimated non-partitioned: 643mb
   Actual non-partitioned: 643 MB
   Estimated partitioned and clustered: 400mb
   Actual partitioned and clustered: 143mb

	SELECT count(*) FROM trips_data_all.fhv_tripdata_cluster
	WHERE DATE(pickup_datetime) BETWEEN '2019-01-1' AND '2019-03-31'
	AND dispatching_base_num IN ('B00987', 'B02060', 'B02279');

5. Can cluster on dispatching_base_num. Because it is a string cannot partition. SR_Flag is an integer in the table I created so it could be used as a partition.

6. No improvement

7. Columnar