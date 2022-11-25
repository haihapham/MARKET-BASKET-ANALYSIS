# MARKET BASKET ANALYSIS - SQL SERVER

## 1. OVERVIEW.
Market Basket Analysis (MBA) also known as association rule learning or affinity analysis, is a data mining technique that can be used in various fields, such as marketing, bioinformatics, education field, nuclear science, etc. The main aim of MBA in marketing is to provide information to the retailer to understand the purchase behavior of the buyer, which can help the retailer in correct decision-making (Kaur & Kang, 2016). <br>
*The objective of this analysis is to determine when customers have 2 certain products in their shopping cart or basket, which items they will be likely to purchase with.*

## 2. MARKET BASKET ANALYSIS WITH ADVENTUREWORKS2019 DATABASE.
- The AdventureWorksDW21019 sample database of Microsoft SQL Server includes the following tables:
![image](https://user-images.githubusercontent.com/118675548/203899917-419371f3-7ccf-47df-949a-1be490815064.png)<br>
- <b>FactInternetSales</b> table, recording sales through internet channel, will be mainly used to analyze our basket with two attributes: <i>SalesOrderNumber</i> and <i>ProductKey</i>.
<pre><code>SELECT TOP 10 
	SalesOrderNumber
	, ProductKey
FROM FactInternetSales;
</code></pre>
- <b><i>Result:</b></i>
<br>![image](https://user-images.githubusercontent.com/118675548/203926346-74cf1315-fb7f-49b5-ba7f-4f7e16b8f493.png)

### 2.1. Query order with at least 3 different products.<br>
<pre><code>SELECT 
    SalesOrderNumber
    , COUNT(ProductKey) AS NumberofProducts
FROM FactInternetSales
GROUP BY SalesOrderNumber
HAVING COUNT(ProductKey) >= 3;
</code></pre>
<b><i>We have the following result:</b></i><br>
![image](https://user-images.githubusercontent.com/118675548/203928381-5c8f43d9-f74e-4dc4-88e1-c477aa1895b7.png)

### 2.2. Query Sales Order Number and Product Key for these above orders.<br>
<pre><code>WITH OrderList AS
(
SELECT
        SalesOrderNumber
        , COUNT(ProductKey) AS NumberofProducts
    FROM FactInternetSales
    GROUP BY SalesOrderNumber
    HAVING COUNT(ProductKey) >= 3
)
SELECT
    OrderList.SalesOrderNumber
    , InSales.ProductKey
FROM OrderList
JOIN FactInternetSales AS InSales 
ON Orderlist.SalesOrderNumber = InSales.SalesOrderNumber;
</code></pre>
<b><i>We have the following result:</b></i><br>
![image](https://user-images.githubusercontent.com/118675548/203927974-f9978368-3c01-4298-af18-fd8a2806f94d.png)

### 2.3. List out baskets of 3 products in the same order.<br>
<pre><code>WITH OrderList AS
(
    SELECT
        SalesOrderNumber
        , COUNT(ProductKey) AS NumberofProducts
    FROM FactInternetSales
    GROUP BY SalesOrderNumber
    HAVING COUNT(ProductKey) >= 3
),
OrderInfo AS 
(
    SELECT
        OrderList.SalesOrderNumber
        , InSales.ProductKey
    FROM OrderList
    JOIN FactInternetSales AS InSales 
    ON Orderlist.SalesOrderNumber = InSales.SalesOrderNumber
)

SELECT 
	OrderInfo1.SalesOrderNumber
	, OrderInfo1.ProductKey AS Product1
	, OrderInfo2.ProductKey AS Product2
    , OrderInfo3.ProductKey AS Product3
FROM OrderInfo AS OrderInfo1
JOIN OrderInfo AS OrderInfo2 
    ON OrderInfo2.SalesOrderNumber = OrderInfo1.SalesOrderNumber
JOIN OrderInfo AS OrderInfo3 
    ON OrderInfo3.SalesOrderNumber = OrderInfo1.SalesOrderNumber   
WHERE OrderInfo1.ProductKey < OrderInfo2.ProductKey  
    AND OrderInfo2.ProductKey < OrderInfo3.ProductKey;
</code></pre>
- We self-join 3 CTE tables (tables of Sales Order Number and Product Key) to create a new table with an Order Number and 3 different products in that order.*<br>
- Condition to avoid duplicate - WHERE: <br>
<b>OrderInfo1.ProductKey < OrderInfo2.ProductKey < OrderInfo3.ProductKey</b>.<br>
- <b><i>We have the following result:</b></i><br>
![image](https://user-images.githubusercontent.com/118675548/203928085-5ac75a48-6eae-4cdb-91e7-8ddd869ef5bd.png)

### 2.4. Measure the frequency of the third product with 2 existing first products.<br>
<pre><code>WITH OrderList AS
(
    SELECT
        SalesOrderNumber
        , COUNT(ProductKey) AS NumberofProducts
    FROM FactInternetSales
    GROUP BY SalesOrderNumber
    HAVING COUNT(ProductKey) >= 3
),
OrderInfo AS 
(
    SELECT
        OrderList.SalesOrderNumber
        , InSales.ProductKey
    FROM OrderList
    JOIN FactInternetSales AS InSales 
    ON Orderlist.SalesOrderNumber = InSales.SalesOrderNumber
)

SELECT DISTINCT
	OrderInfo1.ProductKey AS Product1
	, OrderInfo2.ProductKey AS Product2
    , OrderInfo3.ProductKey AS Product3
    , COUNT(OrderInfo3.ProductKey) OVER(PARTITION BY OrderInfo1.ProductKey, OrderInfo2.ProductKey, OrderInfo3.ProductKey) AS Frequency
FROM OrderInfo AS OrderInfo1
JOIN OrderInfo AS OrderInfo2 
    ON OrderInfo2.SalesOrderNumber = OrderInfo1.SalesOrderNumber
JOIN OrderInfo AS OrderInfo3 
    ON OrderInfo3.SalesOrderNumber = OrderInfo1.SalesOrderNumber   
WHERE OrderInfo1.ProductKey < OrderInfo2.ProductKey  
    AND OrderInfo2.ProductKey < OrderInfo3.ProductKey
ORDER BY Frequency DESC;
</code></pre>
<b><i>We have the following result:</b></i><br>
![image](https://user-images.githubusercontent.com/118675548/203928179-5b796dd3-a999-46df-8747-84071b01810d.png)

- From the above result, we can recognize that when there are products number <b>477</b> and <b>478</b> in customers' baskets, they are most likely to buy products number <b>485</b>.<br>
- We can apply MBA in purchasing suggestions for customers or product arrangements in supermarkets to improve overall sales.

## 3. CONCLUSION
SQL can help us to query data needed for Market Basket Analysis by suggesting them the most relevant product besides what they have chose to optimize revenue for the organizations or businesses and make the right business decisions. 

## REFERENCE 
1. Kaur, M. and Kang, S. (2016) “Market basket analysis: Identify the changing trends of Market Data Using Association Rule Mining,” <i>Procedia Computer Science</i>, 85, pp. 78–85. Available at: https://doi.org/10.1016/j.procs.2016.05.180. 
