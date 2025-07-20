# Superstore Dashboard

### Introduction
#### Purpose
The purpose of this project is to analyze retail data using DAX in Power BI and create a dashboard that can be used at the store level to identify sales trends and top categories.

#### Background
By understanding historical sales data, companies can make informed business decisions regarding inventory, marketing, and staffing. The "Revenue" tab provides the ability to compare monthly and yearly revenue to previous years to identify the percent change and track sales goals. The "Products" tab allows stores to see how many units they have sold, what their top categories are by sales volume, and the top-performing items within that category.

### Data
The data used in this dashboard is a synthetic data set that mimics the sales data of a retail superstore. Columns include metadata on customer name, order date, segmentation, geographic information, product name, cost, sales amount, and quantity sold. The full data set can be found at: [Supersotre Data](https://docs.google.com/spreadsheets/d/1bXqJnFs4fgX0l4J2-PBLxfx8TeR_0qQu/edit?gid=1450198110#gid=1450198110)

#### DAX
DAX was used to create new tables and measures for use in the dashboard.

For analyzing revenue year over year, a new table needed to be created for easier visualization of data by date. This was done by extracting information from the order date column in the original data table.

Creating the Date_Table:
```dax
Date_Table = CALENDAR(MIN('Superstore_Sales_Info xlsx - Sheet 1'[Order Date]), MAX('Superstore_Sales_Info xlsx - Sheet 1'[Order Date]))
```
Extracting Day, Week, Month, and Year:
```dax
Day = DAY(Date_Table[Date])

## Week starts on Monday
Week = WEEKNUM(Date_Table[Date], 2)

Month = MONTH(Date_Table[Date])

Year = YEAR(Date_Table[Date])
```

Next, DAX was used to create a revenue measure for analyzing Total Revenue, Total Revenue Last Year, YoY % Change, Monthly Revenue, and Monthly Revenue Last Year. An example of this code is below:

```dax
Revenue YOY % Change = DIVIDE(([Total YTD Revenue] - [Last YTD Revenue]),[Last YTD Revenue])
```

Finally, to create a snapshot visual of the revenue goals, we can use DAX to change the tile color based on the YoY % Change:

```dax
Box Color = IF([Total YTD Revenue] >= [Last YTD Revenue], "#57B956", "#E66C37")
```

### Insights from the Data
Based on the findings in the data, we can see that 2018 is proving to be a profitable year with a YoY % Revenue increase of more than 20%. This is a trend in the other direction from 2016, which ended below the previous year by 5%. We can also see that November and December are the months with the highest sales volume for the previous three years.

Technology is the best category by sales volume, responsible for over $700k in sales. The highest-selling item in the technology category is the Canon Advanced Copier, selling 13,000 units. The worst-performing item is the Eureka Disposable Vacuum Bags.

### Recommendations for Analysis
For future analysis, we can look into customer locations to see if there is a market that outperforms others or specific products that perform well in some markets compared to others. It would also be beneficial to look into items that are frequently purchased together to help marketing or suggested products when shopping online.
