# Lupin-Meds-Power-bI-PROJECT

I am having the data of Lupin Medical with this data. I have created visualizations on the data i.e. the data is showing different sales, profit, cost and various other parameters for products sold out in different countries and their states.

The data is already clean and here I have Calculated the DimDate Table with the help of M Query for getting the Dates Table separately and have created some measures to define the data well. I took this code and created a table with M Query in Advanced Editor.


Here I have created the first few measures with the help of DAX and first all the measures will be taken from "Imp Measures" and will explain them.

Imp Measures:-   
   - Total Sales:- In this measure total sales have been created with the help of SUMX FUNCTION. It is created by multiplying the Quantity and Unit Price of the Sales Table. Sumx is used to return the sum of an expression evaluated for each row in a table. Only the numbers in the column are counted. Blanks, logical values, and text are ignored.
                   
                   Formula:- Total Sales = SUMX('Sales Details','Sales Details'[Quantity] * 'Sales Details'[Unit Price])
                   
   - Total Cost:- It is created by multiplying the Quantity and Unit Cost of the Sales Table and their sum with the help SUMX function.
                   
                   Formula:- Total Cost = SUMX('Sales Details','Sales Details'[Quantity] * 'Sales Details'[ Unit Cost])
                   
   - Total Profit:- It is the difference between TOTAL SALES and TOTAL PROFIT.
                   
                   Formula:- Total Profit = [Total Sales] - [Total Cost]
                   
   - Profit Margin:- It is calculated by dividing Total Profit and Total Sales.
                   
                   Formula:- Profit Margin = DIVIDE([Total Profit],[Total Sales],0)
                   
   - Total Transactions:- It is calculated with the help of COUNTROWS just counting all the data of Sales Table.
                   
                   Formula:- Total Transactions = COUNTROWS('Sales Details')
                   
   - Avg. Daily Sales:- It returns the average of Total Sales using the Date Column of the DimDate table by getting the unique values of Date.
                   
                   Formula:- Avg. Daily Sales = AVERAGEX(VALUES(DimDate[Date]), [Total Sales])
 
 
Time Intelligence:-

   - Last Purchase Date:- It is calculating in proper dATE format which will give the Last Date from OrderDate Column of Sales Details Table by using the LASTDATE function.
                   
                   Formula:- Last Purchase Date = LASTDATE('Sales Details'[OrderDate])
                   
   - Days Since LAST Purchase Date:- The DATEDIFF function is used to calculate the number of interval boundaries between two dates, intervals to use in the form of DAY, MONTH, YEAR etc. In this I have calculated the difference between two dates in the form of the number of days.
                   
                   Formula:- Days Since Last Purchase Made = DATEDIFF([Last Purchase Date], TODAY(), DAY)
                   
   - Sales Last Year:- Here it is calculating total sales of last year with the help of SAMEPERIODLASTYEAR. SAMEPERIODLASTYEAR is used to return a table that contains a column of dates shifted one year back in time from the dates in the specified dates column, in the current context. Dates are taken from the DimDate Table of Date Column.
                   
                   FORMULA:- Sales LAST Year = CALCULATE([Total Sales], SAMEPERIODLASTYEAR(DimDate[Date]))
                   
   - Sales LY:- In this column we are also calculating last year sales by using DATEADD function. DATEADD is used to return a table that contains a column of dates, shifted either forward or backward in time by the specified number of intervals from the dates in the current context. Here we are calculating backward in time with an integer -1 that specifies the number of intervals to subtract from the dates.
                  
                  Formula:- Sales LY = CALCULATE([Total Sales], DATEADD(DimDate[Date],-1,YEAR))
                  
   - Profit LY:- It is same as Sales LY but we are calculating the profit of Last Year.
                 
                 Formula:- Profit LY = CALCULATE([Total Profit], DATEADD(DimDate[Date],-1,YEAR))
                 
   - Sales Variance:- It is calculating the difference between Total Sales and Sales LY.
                 
                 Formula:- Sales Variance = [Total Sales] - [Sales LY]
                 
Year-wise Sales:-

   - Sales 2019:- Here we are calculating total sales of 2019 by filtering the data of 'Year' Column of DimDate Table.
   
                 Formula:- Sales 2019 = CALCULATE([Total Sales], FILTER(DimDate, DimDate[Year] = 2019))
                 
   - Sales 2020:- Here we are calculating total sales of 2020 by filtering the data of 'Year' Column of DimDate Table.
   
                 Formula:- Sales 2020 = CALCULATE([Total Sales], FILTER(DimDate, DimDate[Year] = 2020))
                
   - Sales Difference:- Calculating the difference between between Sales 2019 and Sales 2020.
               
                 Formula:- Sales Difference = 'Year-wise Sales'[Sales 2019] - 'Year-wise Sales'[Sales 2020]
               
Specific Scenarios:-

   - 2019 Autumn Sales:- Calculating Total Sales of AUTUMN season of 2019 by using DATESBETWEEN function. DATESBETWEEN returns a table that contains a column of dates that begins with a specified start date and continues until a specified end date.
                 
                 Formula:- 2019 Autumn SALES = CALCULATE([Total Sales], DATESBETWEEN('Sales Details'[OrderDate], DATE(2019,9,1), DATE(2019,11,31)))
                 
   - 2019 Spring Sales:- It is calculating Total Sales of Spring season of 2019.
                 
                 Formula:- 2019 Spring SALES = CALCULATE([Total Sales], DATESBETWEEN('Sales Details'[OrderDate], DATE(2019,3,1), DATE(2019,5,31)))
                 
   - 2019 Summer Sales:- It is calculating Total Sales of Summer season of 2019.
                 
                 Formula:- 2019 Summer SALES = CALCULATE([Total Sales], DATESBETWEEN('Sales Details'[OrderDate], DATE(2019,6,1), DATE(2019,8,31)))
                 
   - 2019 Winter Sales:  It is calculating Total Sales of Winter season of 2019.
   
                 Formula:- 2019 Winter SALES = CALCULATE([Total Sales], DATESBETWEEN('Sales Details'[OrderDate], DATE(2019,12,1), DATE(2020,2,31)))
   
   - Distinct CID:- It is calculating unique Customer Name Index of Sales Details Table by using DISTINCTCOUNT.
   
                Formula:- Distinct CID = DISTINCTCOUNT('Sales Details'[Customer Name Index])
   
   - Top 10 Customers:- Here we are calculating Total Sales done by the top 10 Customers. Filtering data of Customer Names from Customer Details Table by using VALUES function, VALUES is used when the input parameter is a column name, returns a one-column table that contains the distinct values from the specified column. Duplicate values are removed and only unique values are returned. And RANKX function is used to return the ranking of a number in a list of numbers for each row in the table argument because we will get top 10 customers by using condition for over data.
   
                Formula:- Top 10 Customers = CALCULATE([Total Sales], FILTER(VALUES('Customer Details'[Customer Names]),
                   IF(RANKX(ALL('Customer Details'[Customer Names]), [Total Sales], , DESC) <= 10,
                   [Total Sales], 
                   BLANK())))
   
   - Top 10 Products:- It is calculating the Top 10 Products of the Total Sales by using Product Name from Product Details Table.
 
                Formula:- Top 10 Products = CALCULATE([Total Sales], FILTER(VALUES('Product Details'[Product Name]),
                   IF(RANKX(ALL('Product Details'[Product Name]), [Total Sales], , DESC) <= 10,
                   [Total Sales], 
                   BLANK()))) 
 
Product Details:- There is two new columns created in the table
  - Sales Figure:- It is same as "Total Sales" measure in which SUMX is used return the sum of an expression evaluated for each row in a table. And The RELATEDTETABLE function is used which changes the context in which the data is filtered, and evaluates the expression in the new context that we specify.
  
                Formula:- Sales Figure = SUMX(RELATEDTABLE('Sales Details'),'Sales Details'[Quantity] * 'Sales Details'[Unit Price])
  
  - Sales Segment:- SWITCH CASE is used here for describing the different conditions of SALES FIGURE.
   
                Formula:- Sales Segment = SWITCH(TRUE(),
                'Product Details'[Sales Figure] <= 3000000, "Low Sales",
                'Product Details'[Sales Figure] < 9000000, "Better Sales",
                "High Sales")
   
  - Sales Germany:- I have created a table through DAX function which will show the columns of Sales Details of Country 'Germany'.
  
Now comes the Data Modelling here in data modelling I have modelled the Region and Sales Germany Table from both the sides means the Cross Filter direction is both to get the data from both the sides with Many to one Cardinality.
and rest other Tables are modelled with a Sales Details Table with a Cross Filter Direction One and Many to one Cardinality.
And the Fact Tables are separated from all other tables, so that is didnot get mixed up with them.

Here is the properties of Region and Sales Germany data modelled together which is cross filtered in both direction.

![Capture1](https://user-images.githubusercontent.com/79398731/183100189-472831a6-d10b-47b5-baaf-4c75ed4ad81a.PNG)

And the properties of other data modelling tables which are Cross filtered in one direction.

![Capture2](https://user-images.githubusercontent.com/79398731/183100940-c5ee24c9-8d2d-4eab-8c3b-0864be7b9626.PNG)

![Capture3](https://user-images.githubusercontent.com/79398731/183100972-7ea162f1-98ad-4c10-95f7-70345f3445e4.PNG)

Now we come to visualization of the data.

Here this visualization is done with the help of Area Chart showing total sales and sales of previous year of different Products sold in different Countries.

![Capture4](https://user-images.githubusercontent.com/79398731/183104706-671db4f7-770e-4d72-8f9c-8075e19c2ad6.PNG)

![Capture5](https://user-images.githubusercontent.com/79398731/183104733-2f21b8ff-3d9b-4275-9f1e-7a44a22a2ff9.PNG)

This is Line and Stacked Column Chart described with Total Sales and Total Profit in different States, where Total Sales is described in the Column format and Total Profit in Line format.

![Capture6](https://user-images.githubusercontent.com/79398731/183107270-ba3a9f00-e0bc-4599-ade6-1b250d2e5967.PNG)

In this visualization it is showing Total Cost of different Products sold out in different countries through Pie Chart

![Capture8](https://user-images.githubusercontent.com/79398731/183109236-cd705282-3b9c-426f-9388-a721e309d315.PNG)

And here total sales, total profit and total cost is described in the card format with a filter applied to a Country Column.

![Capture7](https://user-images.githubusercontent.com/79398731/183108015-c966e8b5-b421-4d89-90d8-e46bedcbbd0e.PNG)


![Dashboard1](https://user-images.githubusercontent.com/79398731/183108206-7995fa35-366c-4fe2-8fff-2dd9c38f93d2.PNG)

Here in this visualization of Total Cost by Product Names if we select any part of it which is just a Product Name and right click on it, then will go for Drill Through --> TC Country it will navigate us to TC Country Page which is Total Cost Country Page.
There we can see the Total Cost of that particular Product selected in Page 1 in different Countries because in TC Country page Product Name is used as Drill Through Option.

![Capture8](https://user-images.githubusercontent.com/79398731/183112901-a91bf529-339e-400b-bc22-bf5c1438923d.PNG)                      ![dashboard 2](https://user-images.githubusercontent.com/79398731/183113067-8ba247bc-9c76-43a9-b887-0fde2ecbf5b6.PNG)


Page 4:- 
In this visualization we are showing sales occured in different seasons like Spring, Summer,Autumn and Winter.

![Capture9](https://user-images.githubusercontent.com/79398731/183128359-5064e399-bd34-402c-ba59-c1b92192329a.PNG)


In this visualization we can see that sales more than 9 Lakh is High SALES which mostly occurred, sales between 3 Lakh and 9 Lakh is Better Sales and less than 3 lakh is Low Sales.

![Capture10](https://user-images.githubusercontent.com/79398731/183128490-4a421f7a-8a3a-4abe-a926-1170e4dcc9a8.PNG)


Here we are showing Top 10 Customers who did maximum Sales.

![Capture11](https://user-images.githubusercontent.com/79398731/183128623-52c23bc7-a737-4b31-ba2b-3f60c0e72419.PNG)


Here we are showing Top 10 Products whose sales are very high.

![Capture12](https://user-images.githubusercontent.com/79398731/183128778-00cb2726-08b5-4e3f-a442-03eb9a835857.PNG)

Dashboard:-

![Dashboard3](https://user-images.githubusercontent.com/79398731/183128897-95e5cd7a-e0cf-4687-944f-e58a6aafb62c.PNG)


Page3:- 


Here in this visualization If I will Ctrl+click on Sales Button it will show us the Total Sales done by the customers, if I go for Profit Button it wil show us Total Profit we made on different Products and Show All will everything in the dashboard.

Clicked on Sales Button:-

![Capture13](https://user-images.githubusercontent.com/79398731/183129960-59ea337d-3b06-4fa6-81e1-2f4f026938e3.PNG)


Clicked on Profit Button:-

![Capture14](https://user-images.githubusercontent.com/79398731/183130072-19ebd0a8-4033-4443-b191-84946b0b6e51.PNG)


Clicked on Show All Button:-

![Capture15](https://user-images.githubusercontent.com/79398731/183130191-9b75bf0f-6769-4073-a401-2b25336d8f04.PNG)


We can use Bookmark from View Tab for different Dashboards to save it and use it later for giving presentations on the data.
Some Bookmarks are presented here which I have used.
Few examples:-

![Capture16](https://user-images.githubusercontent.com/79398731/183131462-64e53197-8780-4f5c-bd54-e9f9d4f23ee3.PNG)


![Capture17](https://user-images.githubusercontent.com/79398731/183131581-2162cea9-ff5f-4020-923a-700828511941.PNG)


And now I END my presentation here only. I will add the data and files through which you can get more references for what I have done.
