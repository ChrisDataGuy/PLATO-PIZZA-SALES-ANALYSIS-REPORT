# PLATO-PIZZA-SALES-ANALYSIS-REPORT

![overview_2](https://github.com/ChrisDataGuy/PLATO-PIZZA-SALES-ANALYSIS-REPORT/assets/109347195/1750170d-c689-4422-81e0-76d211aff960)

## Introduction
This is a Power BI project on sales analysis of an imaginary store PLATO’S PIZZA STORES. Things are going well at Plato's, but there's room for improvement. They  have been collecting transactional data for the  year -2015, but really haven't been able to put it to good use. They look forward to an analysis of the data and a report to help them find opportunities to drive more sales and work more efficiently.

## Statement of the Problem
The manager of Plato’s pizza – Mr. Mario Maven hopes to use the data provided to answer the following questions:
- How much money did we make this year? Can we identify any seasonality in the sales?
- What's our average order value?
- What days and times do we tend to be busiest?
- What are our best and worst selling pizzas?

## Skills Demonstrated
The skills demonstrated in this project included:
-	Data Cleaning and transformation
-	Data modelling
-	Data visualization
-	Storytelling with data
-	DAX

## Data Sourcing
The dataset used for this project can be found [here](https://drive.google.com/drive/folders/1sT5AReif21UXjW1kICtZPrBb8yshNSOs)
This dataset contains 4 tables in CSV format.
- The Orders table contains the date & time that all table orders were placed.
- The Order Details table contains the different pizzas served with each order in the Orders table, and their quantities.
- The Pizzas table contains the size and price for each distinct pizza in the Order Details table, as well as its broader pizza type.
- The Pizza Types table contains details on the pizza types in the Pizzas table, including their name as it appears on the menu, the category it falls under, and its list of ingredients.

## Data Transformation and Cleaning
The following data transformation steps were taken:
-	All tables were checked for duplicates and empty rows, none was found. 
-	Data types were set to the right formats.
-	The hour of the day column was created from the date in the _orders_ table, using the EXTRACT function on the time column.
-	_The Pizza_type_ and _Pizzas tables_ were merged to get a comprehensive _Pizza details_ table that contained the pizza_id, Pizza_type_id, size, price, name, category and ingredients.
-	In the _Pizza details_ tables, in order to have distinct Pizza names that comprised of the pizza name and a suffix of the the size, a DAX formula was used to CONCATENATE the name and size columns. The DAX formula used: 
`Pizza Name = CONCATENATE(Pizza_details[name], CONCATENATE(" - ", Pizza_details[size]))`
-	A Calendar table was created from date column of the _order_details_ table using the following DAX: 
` Calendar Table = CALENDAR(MIN(order_details[Date]),MAX(order_details[Date]))`
The following columns were created in the calendar table using the assigned DAX formulas:
1. Year: `Year = FORMAT('Calendar Table'[Date], "yyyy")`
2. Quarter: `Quarter = FORMAT('Calendar Table'[Date], "Q")`
3. Month: `Month = FORMAT('Calendar Table'[Date], "mmm")`
4. Month Number: `MonthNumber = MONTH('Calendar Table'[Date])`
5. Weekday: `WeekDay = FORMAT('Calendar Table'[Date], "ddd")`
6. Weekday Number: `WeekDayNo = WEEKDAY('Calendar Table'[Date], 2)`

## DAX Measures
The DAX measures created for this project are as follows:
1.	Total Sales: `Total Sales = SUM(order_details[Sales])`
2.	Average order value: `Average Order Value = SUM(order_details[Sales]) / [Distinct Count of Orders]`
3.	YTD Sales: `YTD Sales = TOTALYTD([Total Sales], 'Calendar Table'[Date])`
4.	Order count: `Order Count = DISTINCTCOUNT(order_details[order_id])`
5.	Previous Month sales: `Prev Month Sales = CALCULATE([Total Sales], PREVIOUSMONTH('Calendar Table'[Date]))`
6.	MOM Variance: `MoM Variance = [Total Sales] - [Prev Month Sales]`
7.	% MOM Variance: `% MoM Variance = DIVIDE([MoM Variance], [Prev Month Sales], 0)`

## Data Modelling
Table relationships were manually detected and below is the schema of the data model:

![MODEL](https://github.com/ChrisDataGuy/PLATO-PIZZA-SALES-ANALYSIS-REPORT/assets/109347195/3e209f48-ac7e-4bb0-8d91-304eb41f7d2c)

## Data Analysis and Visualization
#### Top Metrics
Plato’s pizza had a total pizza sales of $817,860.05 for the year 2015 and an average order value of $38.31. Plato’s pizza generated a total of 21,350 unique orders that culminated in the sale of 49,574 packs of pizzas.
Here is a picture of these metrics:

![METRICS](https://github.com/ChrisDataGuy/PLATO-PIZZA-SALES-ANALYSIS-REPORT/assets/109347195/e22d433f-8142-4cf4-9c15-9ccfb6dcfca3)

#### Sales Trend
The daily and hourly sales trend answers the business question: **What days and times do we tend to be busiest?**
Daily sales trend showed that sales peaked on **FRIDAYS** which is reflected in the average order per day table. Daily, pizza sales showed a bimodal distribution, Sales peaked at **12:00 PM - 1:00 PM ( typically LUNCH TIME) and again at 5:00 PM - 6:00 PM**. The **FIVE CHEESE PIZZA - LARGE SIZE** recorded the most sales in the afternoon peak sales period between 12:00 PM and 12:59 PM, with 195 orders. The **THAI CHICKEN PIZZA - LARGE SIZE** was also the most ordered and sold between 1:00 PM and 1:59 PM with a peak of 179 orders and the most sold and ordered during the evening peak between 5:00 PM and 6:59 PM, peaking at 158 orders at that period. Data did not provide the customer demographics to ascertain the reason behind sales surge and preferences at these times of the day. 
The charts below provides pictorial insight:

Sales trend by weekday | Sales trend by hour of the day
---- | ---
![Day of the week sales trend](https://github.com/ChrisDataGuy/PLATO-PIZZA-SALES-ANALYSIS-REPORT/assets/109347195/9f1235f3-8f54-4379-9149-35733c6fddeb) | ![hour of the day](https://github.com/ChrisDataGuy/PLATO-PIZZA-SALES-ANALYSIS-REPORT/assets/109347195/f5ad7d45-e194-4260-929e-5e0ed08956d4)

The monthly sales trend chart answers the business question: **How much money did we make this year?** Can we identify any seasonality in the sales?
Sales trend showed a steady up and down movement between January and June, peaked slightly in July, then a dip in sales occurred between August and October, then a surge in sales in November. No seasonality can be spotted across the months as average sales between months remained the same.
For additional sales insight, attached to the chart is a card showing the Year-to-date sales value, which provides insight on the YTD sales as well as helps to understand how much sales PLATO’S PIZZAS must have accumulated in sales at any point in the year by selecting the month on the chart. For instance, by clicking on the JULY data point, the value is shown to be $486K, which is the total sales from January to July.
See chart below:

![MONTHLY SALES TREND](https://github.com/ChrisDataGuy/PLATO-PIZZA-SALES-ANALYSIS-REPORT/assets/109347195/0ab72956-ceb6-4122-9224-b7e293f959f2)

Sales growth vs previous month chart provides insight on the growth of sales of current month vs previous month. Highest growth versus previous month occurred in November with a +10% growth versus October, December had the worst versus previous month growth of -8% versus November. Charts are seen below:

![Mom growth main](https://github.com/ChrisDataGuy/PLATO-PIZZA-SALES-ANALYSIS-REPORT/assets/109347195/35fce834-08b1-4261-b3d5-ce7b065fe113)

![sales insights monthly](https://github.com/ChrisDataGuy/PLATO-PIZZA-SALES-ANALYSIS-REPORT/assets/109347195/ee81fa77-24d9-49e8-a080-c9ded551dc79)

#### Top 5 and Bottom 5 Pizzas
The following charts answers the question: **What are our best and worst selling pizzas?**

Top performing Pizza in sales and average order value is the **THAI CHICKEN PIZZA - LARGE SIZE** recording total sales of **$29K** and average order value of **$1.4**. The **MEXICANA PIZZA - SMALL SIZE** recorded the least sales. The **BIG MEAT PIZZA - SMALL SIZE** attracted the most orders at **1,811 orders**.
See charts below:

Top 5 Pizzas | Bottom 5 Pizzas
--- | ---
![top 5 pizzas](https://github.com/ChrisDataGuy/PLATO-PIZZA-SALES-ANALYSIS-REPORT/assets/109347195/d43c73ae-e24c-4b46-aed1-78e360c822c3) | ![bottom 5 pizzas](https://github.com/ChrisDataGuy/PLATO-PIZZA-SALES-ANALYSIS-REPORT/assets/109347195/804386d4-5c82-418d-8e51-3c2490f011a9)

Top 20 Pizzas by Customer orders generated:

![TOP 20 BY ORDERS](https://github.com/ChrisDataGuy/PLATO-PIZZA-SALES-ANALYSIS-REPORT/assets/109347195/3dad840a-898f-451d-a5ca-366a821003e7)

#### Sales contribution by Pizza Category:
Difference in sales contribution by pizza category was not too wide, top category - **CLASSIC PIZZAS** had a 26.9% contribution to total sales while the least category - **VEGGIE PIZZAS** had a 23.7% contribution to total sales.

![sales cont by pizza category](https://github.com/ChrisDataGuy/PLATO-PIZZA-SALES-ANALYSIS-REPORT/assets/109347195/dd4d00b9-9825-4c2d-963c-24c9a3c4fffe)

![pizza category insights](https://github.com/ChrisDataGuy/PLATO-PIZZA-SALES-ANALYSIS-REPORT/assets/109347195/efd30b59-e11a-4142-8773-01004faf16ab)

#### The tooltip page:
A tooltip page was created to give additional context of the data. The tooltip was for the top performing product. A multi-row card that showed the Sales in Value, Total Quantity sold and Total Order generated. The filters included Month, Weekday, hour of the day and Pizza category. Below is the tooltip page:

![tooltip page](https://github.com/ChrisDataGuy/PLATO-PIZZA-SALES-ANALYSIS-REPORT/assets/109347195/39f54ba2-839b-463a-bd8c-70c9aaa48d29)

## Recommendations:
-	Implementing Pareto's principle, sales and marketing should  focus on the vital few which are the LARGE SIZE pizzas of the **THAI CHICKEN, FIVE CHEESE, FOUR CHEESE & SPICY ITALIAN PIZZAS, and also the SMALL SIZE BIG MEAT PIZZA**  as it recorded the most orders, it must be have been the preference for a certain customer demographics because of the price.
-	Sales department should ensure the continued supply of the **THAI CHICKEN PIZZA - LARGE SIZE and the FIVE CHEESE PIZZA - LARGE SIZE** at the afternoon (12:00 PM - 1:59 PM) and evening (5:00 PM 6:59 PM) peak sales periods.
-	Additional data on customer information such as class, age group and preferences, should be obtained through short surveys to bring more insights to the data and assist in strategizing marketing and sales.


# THANK YOU 😊
