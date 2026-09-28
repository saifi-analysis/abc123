# Exploratory Data Analysis (EDA) Report
## Cab Booking & Ride Analytics
### 1. Project Overview
This project focuses on analysing cab booking data by integrating multiple datasets, cleaning inconsistent records, performing feature engineering, and conducting Exploratory Data Analysis (EDA). The objective is to understand customer behaviour, driver performance, ride patterns, revenue generation, payment trends, and incident analysis through business KPIs and data visualizations.
________________________________________
### 2. Project Objectives
•	Clean and pre-process raw cab booking datasets.
•	Merge multiple datasets into a unified analytical dataset.
•	Handle missing values, duplicate records, and inconsistent data.
•	Perform feature engineering for advanced analysis.
•	Calculate business KPIs.
•	Analyse ride performance, customer behaviour, driver efficiency, and revenue trends.
•	Create professional data visualizations for business reporting.
________________________________________
### 3. Dataset Description
The project consists of five datasets:
Dataset	Description
Customers	Customer details including demographic information.
Drivers	Driver details such as experience and ratings.
Orders	Ride booking information including fare, payment mode, dates, and service type.
Companies	Cab company information.
Incidents	Ride-related incidents and associated fines.
________________________________________
### 4. Libraries Used
•	Pandas
•	NumPy
•	Matplotlib
•	Seaborn
 
________________________________________
### 5. Data Loading
All datasets were imported using the Pandas library and stored into separate Data Frames before pre-processing and analysis.
 
________________________________________
### 6. Data Cleaning
The following pre-processing steps were performed:
•	Removed duplicate records.
 
 
•	Handled missing values using appropriate techniques.
 
 
 
 
 
 
 
 
 
 
 
 
•	Filled missing Order Date values using Forward Fill.
 
 
 
•	Removed rows with missing Customer IDs where required.
 
•	Corrected inconsistent data types.
 
 
 

 
 
•	Converted date columns into date time format.
 
 
 
•	Standardized categorical values.
 
 
 
 
 
 
 
 
 
 
 
 
 
•	Removed unnecessary spaces and formatting issues.
 
 
 
 
 
•	Verified unique values across important columns.
 
 
 
 
 
 
 
 
•	Performed data consistency checks.
________________________________________
### 7. Data Integration
Multiple datasets were merged using common keys to create a single analytical dataset suitable for business analysis.
Merged Tables:
•	Customers
•	Drivers
•	Orders
•	Companies
•	Incidents
 
________________________________________
### 8. Feature Engineering
Additional columns were created to improve business analysis, including:
•	Month
 
•	Age Category
 
•	Fine Status
 
________________________________________
### 9. Business KPIs
The following KPIs were calculated:
•	Total Revenue
 
•	Total Orders
 
•	Total Customers
 
•	Total Drivers
 
•	Total Companies
 
•	Total Distance Travelled
 
•	Average Fare per Ride
 
•	Total Driver Tips
 
•	Average Customer Rating
 
•	Total Fine Amount
 
•	Total Incidents
 
•	Average Driver Experience
 
•	Repeat Customer Rate
 
•	Incident Rate
 
•	Revenue by City
 
•	Most Popular Service Type
 
•	Most Used Payment Method
 
•	Highest Rated Driver
 
•	Highest Revenue Generating Company
 
•	Average Customer Age
 
•	Percentage of Ratings Above 4★
 
________________________________________
### 10. Exploratory Data Analysis
The following visualizations were created:
Revenue by Pickup City
Purpose:
To compare the total revenue generated from different pickup cities and identify the locations contributing the highest business revenue.
 
________________________________________
Ride Distribution by Service Type
Purpose: 
To analyse ride demand across different service types and understand customer preferences for each service.
 
________________________________________
Payment Mode Distribution
Purpose: 
To examine the proportion of rides completed using different payment methods and understand customer payment preferences.
 
________________________________________
Ride Fare Distribution
Purpose: 
To analyse the distribution of ride fares, identify the most common fare range, and observe the overall pricing pattern.
 
________________________________________
Distance vs Ride Price Analysis
Purpose: 
To examine the relationship between travel distance and ride price and evaluate whether fare increases consistently with distance.
 
________________________________________
Ride Distance Distribution
Purpose: 
To analyse the distribution of ride distances and understand common travel patterns among customers.
 
________________________________________
Revenue Trend over Time
Purpose: 
To analyse monthly revenue growth, identify revenue trends, and evaluate business performance over time.
 
________________________________________
Ride Fare Outlier Analysis
Purpose: 
To detect unusually high or low ride fares, identify outliers, and evaluate the variability in fare distribution.
 
________________________________________
Customer Gender Distribution
Purpose: 
To analyse the demographic distribution of customers based on gender and understand the composition of the customer base.
 
________________________________________
Revenue by Cab Company
Purpose: 
To compare the revenue generated by different cab companies and identify the highest-performing company.
 
________________________________________
Driver Count by Cab Company
Purpose:
To compare the number of drivers associated with each cab company and evaluate their operational capacity.
 
________________________________________
Revenue vs Fine Contribution
Purpose: 
To compare total business revenue with the total fine amount and assess the financial impact of ride-related incidents.
 
________________________________________
Fine Amount by Incident Type
Purpose: 
To analyse the total fine amount associated with each incident type and identify incidents resulting in the highest penalties.
 
________________________________________
Customer Distribution by Age Category
Purpose: 
To analyse the number of customers across different age categories and understand the demographic distribution of the customer base.
 
### 11. Conclusion
1.	The company generated ₹7, 39,633 in total revenue, reflecting the overall earnings from all completed rides during the analysis period.
2.	A total of 4,922 rides were completed, indicating strong customer demand and platform activity.
3.	The platform served 2,438 unique customers, showing its customer reach and market penetration.
4.	The company has 500 registered drivers, providing sufficient fleet capacity to meet customer demand.
5.	10 Cab companies are operating on the platform, creating a competitive and diverse service network.
6.	Drivers collectively covered 42,831 km, demonstrating the operational scale of the business.
7.	The average base fare per ride was ₹ 43, representing the typical cost of a trip before additional charges.
8.	Drivers earned ₹52,937 in customer tips, reflecting customer satisfaction and service quality.
9.	The platform maintained an average customer rating of 3★, indicating the overall service experience.
10.	Each completed ride generated an average revenue of ₹150, helping measure revenue efficiency.
11.	A total of 420 incidents were reported, highlighting the need for continuous monitoring of ride safety and service quality.
12.	The company incurred fines worth ₹4, 83,129, representing the financial impact of reported incidents and policy violations.
13.	Sedan was the most preferred service type, receiving the highest number of bookings among all categories.
14.	Drivers have an average experience of 6 years, indicating the overall expertise of the workforce.
15.	The average customer age is 33 years, providing insights into the primary customer demographic.
16.	The platform operates across 17 cities, demonstrating its geographical coverage.
17.	Customers can choose from 9 different service types, offering flexibility for various travel needs.
18.	Net Banking is the most frequently used payment method, suggesting customer preference for this payment option.
19.	Rapido received the highest number of ride bookings, making it the most preferred cab company on the platform.
20.	41% of all rides received ratings of 4★ or higher, indicating a high level of customer satisfaction.
21.	The incident rate is 9%, meaning approximately 9 incidents occurred for every 100 rides.
22.	58 % of customers booked more than one ride, showing customer loyalty and repeat business.
23.	Driver DRVO235 achieved the highest average rating of 4.7★, demonstrating outstanding service quality.
24.	Rapido generated the highest revenue of ₹95,331, making it the top-performing company financially.
25.	Chandigarh has the highest customer base with 199 registered customers, indicating strong demand and a large user presence in this city.
26.	Mumbai has the highest number of drivers (41), suggesting greater driver availability to meet high ride demand and reduce waiting time.
27.	On average, each driver completed 10 rides, reflecting a balanced workload and healthy driver utilization across the platform.
28.	Driver DRV0438 completed 21 rides, making them the second most active driver and highlighting consistently high service activity.


