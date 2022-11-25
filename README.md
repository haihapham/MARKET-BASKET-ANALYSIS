# MARKET BASKET ANALYSIS - SQL SERVER

## 1. OVERVIEW.
Market Basket Analysis (MBA) also known as association rule learning or affinity analysis, is a data mining technique that can be used in various fields, such as marketing, bioinformatics, education field, nuclear science, etc. The main aim of MBA in marketing is to provide information to the retailer to understand the purchase behavior of the buyer, which can help the retailer in correct decision-making (Kaur & Kang, 2016). <br>
*The objective of this analysis is to determine when customers have 2 certain products in their shopping cart or basket, which items they will be likely to purchase with.*

## 2. MARKET BASKET ANALYSIS WITH ADVENTUREWORKS2019 DATABASE.
The AdventureWorksDW21019 sample database of Microsoft SQL Server includes the following tables:
![image](https://user-images.githubusercontent.com/118675548/203899917-419371f3-7ccf-47df-949a-1be490815064.png)<br>
FactInternetSales table, recording sales through internet channel, will be mainly used to analyze our basket with two attributes: SalesOrderNumber and ProductKey.
### 2.1. FactInternetSales<br>
<pre><code>SELECT TOP 10 -- see the first 10 rows of the table
	SalesOrderNumber
	, ProductKey
FROM FactInternetSales;
</code></pre>


## REFERENCE 
1. Kaur, M. and Kang, S. (2016) “Market basket analysis: Identify the changing trends of Market Data Using Association Rule Mining,” <i>Procedia Computer Science</i>, 85, pp. 78–85. Available at: https://doi.org/10.1016/j.procs.2016.05.180. 
