# Flight-Punctuality
The dataset used for this project is punctuality statistics for selected UK airports. There are 12 files for each month of 2021. This data is used for research using a data warehouse &amp; Tableau. 

## Data Warehouse
<img width="530" alt="image" src="https://github.com/Nincy11/Flight-Punctuality/assets/46756664/a9108b31-5463-4560-9d1c-95c94a3d659e">

## SQL Statements
Airport Dimension Table:<br>
CREATE TABLE airport_dim (
    airport_id INT PRIMARY KEY,
    airport_name VARCHAR(255),
    city VARCHAR(255),
    state VARCHAR(255),
    country VARCHAR(255)
);<br>
<br>
Airline Dimension Table:<br>
CREATE TABLE airline_dim (
    airline_id INT PRIMARY KEY,
    airline_name VARCHAR(255),
    airline_code VARCHAR(10)
);<br>
<br>
Flight Schedule Dimension Table:<br>
CREATE TABLE flight_schedule_dim (
    flight_schedule_id INT PRIMARY KEY,
    airline_id INT,
    flight_number VARCHAR(10),
    departure_airport_id INT,
    arrival_airport_id INT,
    scheduled_departure_time TIMESTAMP,
    scheduled_arrival_time TIMESTAMP,
    CONSTRAINT fk_airline_id
        FOREIGN KEY (airline_id)
        REFERENCES airline_dim (airline_id),
    CONSTRAINT fk_departure_airport_id
        FOREIGN KEY (departure_airport_id)
        REFERENCES airport_dim (airport_id),
    CONSTRAINT fk_arrival_airport_id
        FOREIGN KEY (arrival_airport_id)
        REFERENCES airport_dim (airport_id)
);<br>
<br>
Destination Dimension Table:<br>
CREATE TABLE destination_dim (
    destination_id INT PRIMARY KEY,
    destination_city VARCHAR(255),
    destination_state VARCHAR(255),
    destination_country VARCHAR(255)
);<br>

<br>


Date Dimension Table<br>
CREATE TABLE date_dim (
    date_id INT PRIMARY KEY,
    date DATE,
    year INT,
    month INT,
    day INT,
    week_of_year INT,
    day_of_week INT
);<br>

Flight Punctuality Fact Table:<br>
CREATE TABLE flight_punctuality_fact (
    flight_schedule_id INT,
    date_id INT,
    destination_id INT,
    flights_more_than_15_minutes_early_percent DECIMAL(5,2),
    flights_0_to_15_minutes_late_percent DECIMAL(5,2),
    flights_between_16_and_30_minutes_late_percent DECIMAL(5,2),
    flights_between_31_and_60_minutes_late_percent DECIMAL(5,2),
    flights_between_61_and_120_minutes_late_percent DECIMAL(5,2),
    flights_between_121_and_180_minutes_late_percent DECIMAL(5,2),
    flights_between_181_and_360_minutes_late_percent DECIMAL(5,2),
    flights_more_than_360_minutes_late_percent DECIMAL(5,2),
    CONSTRAINT fk_flight_schedule_id
        FOREIGN KEY (flight_schedule_id)
        REFERENCES flight_schedule_dim (flight_schedule_id),
    CONSTRAINT fk_date_id
        FOREIGN KEY (date_id)
        REFERENCES date_dim (date_id),
    CONSTRAINT fk_destination_id
        FOREIGN KEY (destination_id)
        REFERENCES destination_dim (destination_id)
);<br>
<br>
## Visualizations
Visualization 1 <br>
<img width="454" alt="image" src="https://github.com/Nincy11/Flight-Punctuality/assets/46756664/8b26f328-e89a-48a7-8640-b479481d3de1">
<br> Bar graph displaying average delay in mins for all months in 2021 categorised by scheduled or chartered flights.<br>
### Key findings:
The maximum delay in mins appears to be during the months December, October, November, and August. These months fall in the winter and autumn seasons in the UK, and weather conditions can often cause flight delays. For instance, snow, fog, and rain can lead to reduced visibility, which can delay flights. Another reason would be increased passenger traffic. These months coincide with school holidays and festive seasons, which could result in higher passenger traffic. As a result, airlines might schedule more flights, which could lead to congestion and potential delays. Another assumption is that Airlines may be adjusting their operations during these months, such as transitioning from summer to winter schedules, or preparing for the holiday season. Such adjustments could lead to unforeseen delays.

Visualization 2 <br>
<img width="454" alt="image" src="https://github.com/Nincy11/Flight-Punctuality/assets/46756664/0c4d7b5f-e1d2-47f0-aa12-c14326d4500a">
<br>
Highlight table displaying popular months and airlines in UK for the year 2021.
<br>
### Key findings:
The most popular months for travelling were October, December, August and September and the most popular airlines were EasyJet, Ryanair and British Airways.
One of the reasons behind this may be seasonality. The holiday season in December, as well as school breaks in October, November, and August, might lead to an increase in air travel demand, resulting in more flights during these months. In contrast, the months of February, March, and April are not typically associated with holidays or breaks, which might explain why the average number of flights is lower.
Another assumption is that many companies might schedule business meetings or conferences during the holiday season or in the months leading up to it, which could explain the higher average number of flights in December, October, November, and August.

Visualization 3 <br>
<img width="454" alt="image" src="https://github.com/Nincy11/Flight-Punctuality/assets/46756664/c35ad3f7-1d73-42d0-b24a-f3008a3a2c7f">
<br>Multi-bar graph depicting distribution of delays in minuted across different time intervals for each airport in UK.
<br>
### Key findings: 
From this chart, it is evident that flights are always less than 15mins late across all airports. 
This can be because flight schedules are typically designed with built-in buffer times to accommodate unforeseen circumstances such as minor delays due to weather or air traffic control. These buffers often allow airlines to make up for lost time and reduce the likelihood of flights being more than 15 minutes late.
Flights that are significantly delayed may be cancelled instead of being allowed to take off, which would result in fewer flights being classified as more than 360 minutes late.

Dashboard	<br>
<img width="532" alt="image" src="https://github.com/Nincy11/Flight-Punctuality/assets/46756664/d1a18cfa-4044-4c0e-b4e4-d557a3efaa30">
<br> Tableau Dashboard for flight punctuality dataset 2021.<br>

## Key Findings
1.	EasyJet has the least number of delays and cancellations, followed by Ryanair and British Airways. This could suggest that passengers flying with these airlines may have a higher likelihood of arriving on time.
2.	Loganair and British Airways has the most number of delays and cancellations. This could suggest that passengers flying with these airlines may need to plan for potential delays and cancellations more than other airlines.
3.	Best-performing airport is Edinburgh This could suggest that passengers flying to or from these airports may have a higher likelihood of arriving on time and experiencing fewer cancellations.
4.	Worst-performing airport is Birmingham. This could suggest that passengers flying to or from these airports may need to plan for potential delays and cancellations.

## Conclusion
The flight punctuality statistics dataset provides a comprehensive view of the punctuality performance of selected UK airports, airlines, and destinations. The dataset contains a large number of columns, which allows for detailed analysis and insights. One limitation of the dataset is that it only covers one year, which may not be enough to identify long-term trends or patterns. Additionally, the dataset only covers a limited number of airports and airlines, which may not be representative of the entire industry.

Overall, the dataset provides valuable insights into the punctuality performance of UK airports and airlines and can be used for a variety of analytical purposes, including trend analysis, benchmarking, and performance monitoring.


