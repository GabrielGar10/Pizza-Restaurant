## Pizza-Restaurant
  This is a Data Analysis and Visualization Project using the Pizza Restaurant Sales Dataset which was provided on Kaggle. 
Link to Kaggle Dataset Page: https://www.kaggle.com/datasets/shilongzhuang/pizza-sales
This project was done using SQL (Big Query) and Tableau (Tableau Public).
Link to Tableau Public Dashboards: https://public.tableau.com/app/profile/gabriel.garza7892
The "Vizzes" for this project will be named "Pizza Restaurant Dashboards"

## Methodolgy
Data Analysis and Visualization
## Excel Sheet Editing
  Before we begin analysis, there are a few changes I made to the Excel file to make querying and visualization easier. First, I used the TEXT function such as =TEXT(E2,"dddd"), =TEST(E2,"mmmm"), =TEST(E2,"yyyy") to pull the Day Name, Month Name, and Year from the date column. Then, I used the HOUR function to pull only the hour from the time into its own column. Finally, I used an IFS function to group each row into a time period based on the hour column such as =IFS(AND(J2>=6,J2<12)=TRUE,"Morning",AND(J2>=12,J2<18)=TRUE,"Afternoon",AND(J2>=18,J2<24)=TRUE,"Evening")
  
  

## Data Analysis
## Pizza Restaurant Data Analysis and Visualization
## Checking if our dataset loads properly
SELECT 
  *
FROM
  `Pizza.Pizza_Restaurant`;

## What days and times does Plato's Pizza tend to be busiest?
SELECT
  Day,
  SUM(quantity) AS Orders
FROM
  `Pizza.Pizza_Restaurant`
GROUP BY
  Day
ORDER By
  Orders DESC;

SELECT
  Month,
  SUM(quantity) AS Orders
FROM
  `Pizza.Pizza_Restaurant`
GROUP BY
  Month
ORDER BY 
  Orders DESC;

## How many pizzas is the restaurant making during peak hours?
SELECT
  Hour,
  SUM(quantity) AS Orders
FROM
  `Pizza.Pizza_Restaurant`
GROUP BY
  Hour
ORDER BY
  Orders DESC;

SELECT
  CASE
    WHEN Hour >= 6 AND Hour < 12 THEN 'Morning'
    WHEN Hour >= 12 AND Hour < 18 THEN 'Afternoon'
    WHEN Hour >= 18 AND Hour < 24 THEN 'Evening'
    END AS Time_Period,
    SUM(quantity) AS Orders
FROM
  `Pizza.Pizza_Restaurant`
GROUP BY
  Time_Period;

## What are the best and worst selling Pizzas? 
SELECT
  pizza_name AS Pizza_Name,
  pizza_ingredients AS Ingredients,
  SUM(quantity) AS Orders
FROM
  `Pizza.Pizza_Restaurant`
GROUP BY
  Pizza_Name,
  Ingredients
ORDER BY
  Orders DESC;

SELECT 
  pizza_size AS Pizza_Size,
  SUM(quantity) AS Orders
FROM
  `Pizza.Pizza_Restaurant`
GROUP BY
  Pizza_Size
ORDER BY
  Orders DESC;

SELECT
  pizza_category AS Category,
  SUM(quantity) AS Orders
FROM
  `Pizza.Pizza_Restaurant`
GROUP BY
  Category
ORDER BY 
  Orders DESC;

## What's our average order value?
SELECT 
  ROUND(SUM(total_price),2) AS Revenue, 
  SUM(quantity) AS Orders,
    (SELECT
      ROUND((ROUND(SUM(total_price),2)/ SUM(quantity)),2)
     FROM
      `Pizza.Pizza_Restaurant`
    ) AS Average_Order_Value
FROM
  `Pizza.Pizza_Restaurant`;

## How well are we utilizing our seating capacity?
SELECT
  Hour,
  SUM(quantity) AS Pizzas,
  COUNT(DISTINCT(order_id)) AS Total_Orders,
FROM
  `Pizza.Pizza_Restaurant`
WHERE
  order_date = "2015-07-17"
GROUP BY 
  Hour;
  
  
## Data Visualization
![Dashboard 1](https://user-images.githubusercontent.com/120590361/210850871-4b30743f-e01b-4e33-b73d-47ac3b46c77b.png)
![Dashboard 2](https://user-images.githubusercontent.com/120590361/210850884-56f7ff4a-8424-4812-a765-6326eaf3bd73.png)
