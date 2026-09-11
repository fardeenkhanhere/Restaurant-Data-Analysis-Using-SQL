# Restaurant-Data-Analysis-Using-SQL
A practical Restaurant Data Analysis project using SQL to explore customer and food sales data. This project covers 20 questions from basic queries to advanced concepts like JOINs, GROUP BY, HAVING, CTEs, and Window Functions, helping turn raw restaurant data into useful business insights.


Restaurant Data Analysis Using SQL
Customers & Orders Dataset — 20 SQL Questions Solved
In this project, we’ll analyze a restaurant dataset containing customer information and food order details.
The goal is to use SQL to answer practical business questions such as:
Who are our highest-value customers?
Which cities generate the most sales?
What food items are most popular?
Which customer segments spend the most?
How can we rank customers based on their sales?
The project starts with basic SQL concepts and gradually moves toward more advanced topics such as JOINs, GROUP BY, HAVING, CTEs, and Window Functions.

1. Display All Customers
SELECT *
FROM Customers;


What we're doing:
This is the simplest way to explore the customer table. It allows us to see all available columns and records.
Concept: SELECT

2. Display Customer Name, City and Customer Segment
SELECT 
    Customer_Name,
    City,
    Customer_Segment
FROM Customers;


What we're doing:
Instead of displaying every column, we select only the information we need. This makes the result easier to read and is useful when working with large datasets.
Concept: Selecting specific columns.

3. Find All Customers from Mumbai
SELECT *
FROM Customers
WHERE City = 'Mumbai';


What we're doing:
Here, we only want customers who belong to Mumbai. The WHERE clause helps us filter the data based on a specific condition.
Concept: WHERE

4. Find Customers Whose Age Is Greater Than 40
SELECT 
    Customer_ID,
    Customer_Name,
    Age
FROM Customers
WHERE Age > 40;


What we're doing:
We are filtering customers based on their age and displaying only customers who are older than 40.
This is a common example of using comparison operators in SQL.
Concept: Comparison operators (>)

5. Find All Unique Food Categories
SELECT DISTINCT Food_Category
FROM Food_Sales;


What we're doing:
The DISTINCT keyword removes duplicate values. So, if a food category appears hundreds of times, it will still appear only once in our result.
Concept: DISTINCT

6. Find the Total Number of Customers
SELECT COUNT(*) AS Total_Customers
FROM Customers;


What we're doing:
We use COUNT() to find out how many customer records exist in our database.
Concept: COUNT()

7. Find the Total Quantity of Food Sold
SELECT SUM(Quantity) AS Total_Quantity_Sold
FROM Food_Sales;


What we're doing:
Here, we're adding the quantity from all orders to understand the total number of food units sold.
Concept: SUM()

8. Find the Average Rating of All Orders
SELECT 
    ROUND(AVG(Rating), 2) AS Average_Rating
FROM Food_Sales;


What we're doing:
We calculate the average customer rating and use ROUND() to display the result up to two decimal places.
This gives us a quick idea of the overall customer satisfaction level.
Concept: AVG() + ROUND()

9. Find the Top 10 Most Expensive Food Items
SELECT 
    Food_Item,
    Unit_Price
FROM Food_Sales
ORDER BY Unit_Price DESC
LIMIT 10;


What we're doing:
We sort food items from the highest price to the lowest price and then use LIMIT 10 to display only the top 10.
Concept: ORDER BY + LIMIT

10. Find Total Sales for Each Food Category
Here, we calculate sales using:
Sales = Quantity × Unit Price
SELECT 
    Food_Category,
    SUM(Quantity * Unit_Price) AS Total_Sales
FROM Food_Sales
GROUP BY Food_Category
ORDER BY Total_Sales DESC;


What we're doing:
We're calculating total sales for every food category.
First, we calculate the sales value for each order using Quantity × Unit_Price. Then, SUM() adds those values category-wise.
Finally, we sort the categories from highest to lowest sales.
Concept: GROUP BY + SUM() + ORDER BY
Business question: Which food category generates the most sales?

11. Display Customer Names Along With Their Orders
SELECT
    C.Customer_ID,
    C.Customer_Name,
    F.Order_ID,
    F.Order_Date,
    F.Food_Item,
    F.Quantity
FROM Customers C
INNER JOIN Food_Sales F
    ON C.Customer_ID = F.Customer_ID;


What we're doing:
Now we're working with two connected tables: Customers and Food_Sales.
The Customer_ID connects these tables, allowing us to see which orders belong to which customers.
Concept: INNER JOIN + Foreign Key relationship.

12. Display Customer Name, City, Food Item and Order Type
SELECT
    C.Customer_Name,
    C.City,
    F.Food_Item,
    F.Order_Type
FROM Customers C
JOIN Food_Sales F
    ON C.Customer_ID = F.Customer_ID;


What we're doing:
This query combines customer information with their order information.
For example, we can see which customer, from which city, ordered which food item and through which order type.
This is an important query for understanding how two related tables can be analyzed together.
Concept: JOIN

13. Find Total Sales Generated by Each Customer
SELECT
    C.Customer_ID,
    C.Customer_Name,
    SUM(F.Quantity * F.Unit_Price) AS Total_Sales
FROM Customers C
JOIN Food_Sales F
    ON C.Customer_ID = F.Customer_ID
GROUP BY
    C.Customer_ID,
    C.Customer_Name
ORDER BY Total_Sales DESC;


What we're doing:
Here, we're calculating how much revenue each customer has generated.
We join the two tables, calculate Quantity × Unit_Price, group the results customer-wise, and then sort customers from highest to lowest sales.
Concepts:
JOIN + SUM() + GROUP BY + ORDER BY
Business question: Who are our highest-value customers?

14. Find the Top 10 Customers Based on Total Sales
SELECT
    C.Customer_ID,
    C.Customer_Name,
    SUM(F.Quantity * F.Unit_Price) AS Total_Sales
FROM Customers C
JOIN Food_Sales F
    ON C.Customer_ID = F.Customer_ID
GROUP BY
    C.Customer_ID,
    C.Customer_Name
ORDER BY Total_Sales DESC
LIMIT 10;


What we're doing:
This is similar to the previous question, but now we're interested only in the top 10 customers.
This type of analysis can help a business identify its most valuable customers.
Business question: Who are our top 10 customers by revenue?
Concepts: JOIN + GROUP BY + SUM() + ORDER BY + LIMIT

15. Find Total Sales by Customer City
SELECT
    C.City,
    SUM(F.Quantity * F.Unit_Price) AS Total_Sales
FROM Customers C
JOIN Food_Sales F
    ON C.Customer_ID = F.Customer_ID
GROUP BY C.City
ORDER BY Total_Sales DESC;


What we're doing:
Instead of analyzing customers individually, we're now looking at sales city-wise.
This can help a restaurant understand which locations are contributing the most revenue.
Business question: Which city generates the most revenue?
Concepts: JOIN + GROUP BY + SUM() + ORDER BY

16. Find the Average Order Value for Each Customer Segment
SELECT
    C.Customer_Segment,
    ROUND(
        SUM(F.Quantity * F.Unit_Price) / COUNT(DISTINCT F.Order_ID),
        2
    ) AS Average_Order_Value
FROM Customers C
JOIN Food_Sales F
    ON C.Customer_ID = F.Customer_ID
GROUP BY C.Customer_Segment
ORDER BY Average_Order_Value DESC;


What we're doing:
Here, we're comparing the average order value across different customer segments.
We calculate total sales for each segment and divide it by the number of unique orders.
This helps us understand which customer segment tends to spend more per order.
Concept: Multiple aggregate calculations + COUNT(DISTINCT).
Business question: Which customer segment has the highest average order value?

17. Find Customers Who Have Placed More Than 5 Orders
SELECT
    C.Customer_ID,
    C.Customer_Name,
    COUNT(F.Order_ID) AS Total_Orders
FROM Customers C
JOIN Food_Sales F
    ON C.Customer_ID = F.Customer_ID
GROUP BY
    C.Customer_ID,
    C.Customer_Name
HAVING COUNT(F.Order_ID) > 5
ORDER BY Total_Orders DESC;


What we're doing:
We first count the number of orders placed by each customer.
Then, using HAVING, we keep only customers who have placed more than 5 orders.
This is a great example of the difference between WHERE and HAVING.
Remember:
WHERE filters individual rows before grouping.
HAVING filters grouped results after aggregation.
Business question: Which customers are placing orders frequently?

18. Find the Most Popular Food Item
SELECT
    Food_Item,
    SUM(Quantity) AS Total_Quantity_Sold
FROM Food_Sales
GROUP BY Food_Item
ORDER BY Total_Quantity_Sold DESC
LIMIT 1;


What we're doing:
We're measuring popularity based on the total quantity sold.
We group the orders by food item, add up the quantities, sort them from highest to lowest, and return the number-one item.
Business question: Which food item has sold the most units?
Concept: Aggregation + Ranking.

19. Find the Highest-Spending Customer Using a CTE
WITH Customer_Sales AS
(
    SELECT
        C.Customer_ID,
        C.Customer_Name,
        SUM(F.Quantity * F.Unit_Price) AS Total_Sales
    FROM Customers C
    JOIN Food_Sales F
        ON C.Customer_ID = F.Customer_ID
    GROUP BY
        C.Customer_ID,
        C.Customer_Name
)


SELECT
    Customer_ID,
    Customer_Name,
    Total_Sales
FROM Customer_Sales
ORDER BY Total_Sales DESC
LIMIT 1;


What we're doing:
Here, we're introducing a more advanced SQL concept called a CTE — Common Table Expression.
The CTE first calculates total sales for every customer. We then use that result in the main query to find the customer with the highest total sales.
You can think of a CTE as creating a temporary result that makes a complex query easier to understand and manage.
Concept: CTE using the WITH clause.

20. Rank Customers Based on Their Total Sales
This is an advanced Data Analyst and SQL interview-style question.
WITH Customer_Sales AS
(
    SELECT
        C.Customer_ID,
        C.Customer_Name,
        C.Customer_Segment,
        SUM(F.Quantity * F.Unit_Price) AS Total_Sales
    FROM Customers C
    JOIN Food_Sales F
        ON C.Customer_ID = F.Customer_ID
    GROUP BY
        C.Customer_ID,
        C.Customer_Name,
        C.Customer_Segment
)


SELECT
    Customer_ID,
    Customer_Name,
    Customer_Segment,
    Total_Sales,
    RANK() OVER (
        ORDER BY Total_Sales DESC
    ) AS Sales_Rank
FROM Customer_Sales
ORDER BY Sales_Rank;


What we're doing:
This takes our analysis one step further.
First, the CTE calculates total sales for each customer. Then, the RANK() window function assigns a sales rank based on each customer's total sales.
The customer with the highest sales gets Rank 1, the next gets Rank 2, and so on.
An important advantage of RANK() is that customers with the same sales value receive the same rank.
Concepts:
CTE
JOIN
GROUP BY
SUM()
RANK()
Window Functions
Business question: How do customers rank based on the revenue they generate?

Key SQL Concepts Covered
By completing these 20 questions, we have worked with several important SQL concepts:
Level
SQL Concepts
Beginner
SELECT, WHERE, DISTINCT
Basic Analysis
COUNT(), SUM(), AVG(), ROUND()
Sorting & Filtering
ORDER BY, LIMIT
Aggregation
GROUP BY, Aggregate Functions
Intermediate
INNER JOIN, Foreign Keys
Advanced Filtering
HAVING, COUNT(DISTINCT)
Advanced SQL
CTE (WITH)
Advanced Analytics
RANK() and Window Functions

Final Project Takeaway
This project demonstrates how SQL can be used to turn raw customer and order data into meaningful business insights.
Starting from simple data retrieval, we gradually moved toward customer segmentation, revenue analysis, top-customer identification, order-frequency analysis, CTEs, and customer ranking.

