/* Abstract: This project analyzed the San Francisco bike share data from Google Big Query and provided recommendations to 
improve the service, by answering crucial business questions. */
--STATIONS:
--Which stations have the highest number of docks?
SELECT
 s.station_id,
 i.name,
 i.region_id,
 i.capacity AS total_docks,
 s.num_docks_available AS docks_available,
 s.num_bikes_available AS bikes_available
FROM
 bigquery-public-data.san_francisco_bikeshare.bikeshare_station_status AS s
INNER JOIN
 bigquery-public-data.san_francisco_bikeshare.bikeshare_station_info AS i
USING
 (station_id)
GROUP BY
 s.station_id,
 i.name,
 i.region_id,
 i.capacity,
 s.num_docks_available,
 s.num_bikes_available
ORDER BY
 i.capacity DESC
LIMIT
 5;

--Which stations are empty (Many docks available)?
SELECT
 s.station_id,
 i.name,
 i.region_id,
 i.capacity AS total_docks,
 s.num_docks_available AS docks_available,
 s.num_bikes_available AS bikes_available
FROM
 bigquery-public-data.san_francisco_bikeshare.bikeshare_station_status AS s
INNER JOIN
 bigquery-public-data.san_francisco_bikeshare.bikeshare_station_info AS i
USING
 (station_id)
GROUP BY
 s.station_id,
 i.name,
 i.region_id,
 i.capacity,
 s.num_docks_available,
 s.num_bikes_available
ORDER BY
 s.num_docks_available DESC
LIMIT
 5;
 
--Most famous stations where people start their trip
SELECT
 start_station_name,
 COUNT(*) AS count
FROM
 bigquery-public-data.san_francisco_bikeshare.bikeshare_trips
GROUP BY
 start_station_name
ORDER BY
 Count DESC
LIMIT
 5;
 
--Most famous stations where people end their trip
SELECT
 end_station_name,
 COUNT(*) AS count
FROM
 bigquery-public-data.san_francisco_bikeshare.bikeshare_trips
GROUP BY
 end_station_name
ORDER BY
 count DESC
LIMIT
 5;
 
/* Station IDs 460, 412, 37, 21, 50 have the highest number of docks across the whole city and also seem to have a fairly 
good balance between the number of docks/number of bikes available at a given point.
Station IDs 350, 363, 75, 467, 197 appear to be empty as shown by the high availability of docks. Worth pointing out that 
Station IDs 350 and 197 also have a very few bikes available at a given point, highlighting the higher demand for bikes in 
these stations, compared to the other 3, who seem to have a fair balance between the empty docks as well as available bikes, 
highlighting low demand.
The most famous stations where people start their trip or end their trip appear to be the same, although these stations don’t 
appear on the list for stations with maximum number of docks. Highlighting the need for more docks in these stations to 
accommodate more people. */



--TRIPS:
--Most famous trips
SELECT
 start_station_name,
 end_station_name,
 COUNT(*) AS count
FROM
 bigquery-public-data.san_francisco_bikeshare.bikeshare_trips
GROUP BY
 start_station_name,
 end_station_name
ORDER BY
 count DESC
LIMIT
 5;
 
--Most famous round trips
SELECT
 start_station_name,
 end_station_name,
 COUNT(*) AS count
FROM
 bigquery-public-data.san_francisco_bikeshare.bikeshare_trips
WHERE
 start_station_name = end_station_name
GROUP BY
 start_station_name,
 end_station_name
ORDER BY
 count DESC
LIMIT
 5;
 
/* The top 3 most famous trips are between Harry Bridges Plaza (Ferry Building) to Embarcadero at Sansome, San Francisco 
Caltrain 2 (330 Townsend) to Townsend at 7th and 2nd at Townsend to Harry Bridges Plaza (Ferry Building). Most of these 
stations are also the favorites to start or end a trip, as seen before.
The most famous round trips occur at Embarcadero at Sansome, Harry Bridges Plaza (Ferry Building) and The Embarcadero at 
Sansome St. Since these happen to be the iconic spots of the city, people are probably renting bikes to go around these 
stations. Although frequently used, none of these stations appear on the list for having the highest number of docks. */




--USERS:
--Which subscriber type takes more trips
SELECT
 subscriber_type,
 COUNT(*) AS count
FROM
 bigquery-public-data.san_francisco_bikeshare.bikeshare_trips
GROUP BY
 subscriber_type;
 
--Usage by subscriber type
SELECT
 subscriber_type,
 ROUND(AVG(duration_sec),2) AS average_duration
FROM
 bigquery-public-data.san_francisco_bikeshare.bikeshare_trips
GROUP BY
 subscriber_type
ORDER BY
 average_duration DESC;
 
/* The Subscribers are ~5 times more in number compared to the Customers, indicating that most users are registered and 
probably use the bikes for a long term, compared to the one-time users.
Although its interesting to see that a one-time Customer spends on average ~5 times more duration on the bikes, compared to 
the Subscriber. Probably the case being subscribers know the whereabouts of the city better than the Customer, who would be 
a temporary visitor to the city. */



/* RECOMMENDATION:
Based on the analysis, it is highly recommended to increase the number of docks at stations Harry Bridges Plaza (Ferry 
Building), Embarcadero at Sansome and San Francisco Caltrain 2. Since the data shows that these stations are mostly in high 
demand from users and also have the highest count of trips to and from them. These areas also happen to be the epicenter of 
tourism in the city, where visitors from around the world come to explore the rich heritage of San Francisco. Also to note 
that, these areas have broad and well maintained bike lanes, making it easier and safer for bikers to go around exploring. 
Increasing the number of docks would mean higher number of bike availability, translating to more usage of the bikes. */

--APPENDIX

--Empty Stations
SELECT
 COUNT(*) AS empty_station
FROM
 bigquery-public-data.san_francisco_bikeshare.bikeshare_station_status
WHERE
 num_bikes_available = 0;
 
 
--Stations at capacity
SELECT
 COUNT(*) AS station_at_capacity
FROM
 bigquery-public-data.san_francisco_bikeshare.bikeshare_station_status
WHERE
 num_docks_available = 0;
 
 
--Out of service stations
SELECT
 COUNT(*) AS out_of_service
FROM
 bigquery-public-data.san_francisco_bikeshare.bikeshare_station_status
WHERE
 is_renting = FALSE
 AND is_returning = FALSE;
 
 
--Fill rate of each station
SELECT
 s.station_id,
 s.num_bikes_available/i.capacity AS fill_rate
FROM
 bigquery-public-data.san_francisco_bikeshare.bikeshare_station_status AS s
INNER JOIN
 bigquery-public-data.san_francisco_bikeshare.bikeshare_station_info AS i
USING
 (station_id)
WHERE
 i.capacity > 0
ORDER BY
 fill_rate DESC;
 
 
--Average trip duration
SELECT
 ROUND(AVG(duration_sec),2) AS average_duration
FROM
 bigquery-public-data.san_francisco_bikeshare.bikeshare_trips;
 
 
--Shortest trip
SELECT
 ROUND(MIN(duration_sec),2) AS shortest_trip
FROM
 bigquery-public-data.san_francisco_bikeshare.bikeshare_trips;
 
 
--Longest trip
SELECT
 MAX(duration_sec) AS longest_trip
FROM
 bigquery-public-data.san_francisco_bikeshare.bikeshare_trips;
 
 
--Total hours of usage per bike
SELECT
 bike_number,
 SUM(duration_sec) AS total_usage
FROM
 bigquery-public-data.san_francisco_bikeshare.bikeshare_trips
GROUP BY
 bike_number
ORDER BY
 total_usage DESC;
 
 
--Number of trips per day
SELECT
 EXTRACT(DATE
 FROM
 start_date) AS days,
 COUNT(*) AS number_of_trips
FROM
 bigquery-public-data.san_francisco_bikeshare.bikeshare_trips
GROUP BY
 days
ORDER BY
 days;
 
 
--Which hours of the day does usage peak on weekdays?
SELECT
 EXTRACT(HOUR
 FROM
 start_date) AS hour,
 COUNT(*) AS number_of_trips
FROM
 bigquery-public-data.san_francisco_bikeshare.bikeshare_trips
WHERE
 (EXTRACT(WEEK
 FROM
 start_date)) NOT IN (0,
 1)
GROUP BY
 hour
ORDER BY
 number_of_trips DESC;
 
 
--Which hours of the day does usage peak on weekends?
SELECT
 EXTRACT(HOUR
 FROM
 start_date) AS hour,
 COUNT(*) AS number_of_trips
FROM
 bigquery-public-data.san_francisco_bikeshare.bikeshare_trips
WHERE
 (EXTRACT(WEEK
 FROM
 start_date)) IN (0,
 1)
GROUP BY
 hour
ORDER BY
 number_of_trips DESC;
