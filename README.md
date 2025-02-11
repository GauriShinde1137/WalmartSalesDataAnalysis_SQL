# WalmartSalesDataAnalysis_SQL

use walmartsd
SELECT *FROM walmartsalesdata;


#Generic Questions:
#1. How many unique cities does the data have?

SELECT City, COUNT(*) FROM walmartsalesdata  GROUP BY City;

#2.In which city is each branch?

SELECT branch, city, COUNT(*) AS count FROM walmartsalesdata GROUP BY branch, city;


#Product Analysis:
#1. How many unique product lines does the data have?

SELECT `Product line`, COUNT(*) AS count FROM walmartsalesdata GROUP BY `Product line`;

#2. What is the most common payment method?

SELECT Payment, COUNT(*) AS count 
FROM walmartsalesdata 
GROUP BY Payment
ORDER BY count DESC 
LIMIT 1;

#3. What is the most selling product line?

SELECT `Product line`, SUM(quantity) AS total_sold 
FROM walmartsalesdata 
GROUP BY `Product line` 
ORDER BY total_sold DESC 
LIMIT 1;

#4. What is the total revenue by month?

SELECT MONTH(date) AS month, SUM(total) AS Total_Revenue 
FROM walmartsalesdata 
GROUP BY MONTH(date) 
ORDER BY month;

#5. What month had the largest COGS?

SELECT MONTH(date) AS month, SUM(cogs) AS total_cogs 
FROM walmartsalesdata 
GROUP BY MONTH(date) 
ORDER BY total_cogs DESC 
LIMIT 1;

#6. What product line had the largest revenue?

SELECT `Product line`, SUM(total) AS total_revenue 
FROM walmartsalesdata 
GROUP BY `Product line` 
ORDER BY total_revenue DESC 
LIMIT 1;

#7. What is the city with the largest revenue?

SELECT city, SUM(total) AS total_revenue 
FROM walmartsalesdata 
GROUP BY city 
ORDER BY total_revenue DESC 
LIMIT 1;

#8. What product line had the largest VAT?

SELECT `Product line`, SUM(`Tax 5%`) AS total_vat 
FROM walmartsalesdata 
GROUP BY `Product line`
ORDER BY total_vat DESC 
LIMIT 1;

#9. Fetch each product line and add a column to classify them as "Good" or "Bad" based on average sales.

SELECT `Product line`, 
       CASE 
           WHEN AVG(quantity) > (SELECT AVG(quantity) FROM walmartsalesdata) THEN 'Good' 
           ELSE 'Bad' 
       END AS category
FROM walmartsalesdata 
GROUP BY `Product line`;

#10. Which branch sold more products than the average product sold?

SELECT branch, SUM(quantity) AS total_sold 
FROM walmartsalesdata 
GROUP BY branch 
HAVING total_sold > (SELECT AVG(quantity) FROM walmartsalesdata);

#11. What is the most common product line by gender?

SELECT gender, `Product line`, COUNT(*) AS total_sold 
FROM walmartsalesdata 
GROUP BY gender, `Product line` 
ORDER BY gender, total_sold DESC;

#12. What is the average rating of each product line?

SELECT `Product line`, AVG(rating) AS avg_rating 
FROM walmartsalesdata 
GROUP BY `Product line` 
ORDER BY avg_rating DESC;


#Sales Analysis
#1. Number of sales made in each time of the day per weekday

SELECT DAYNAME(date) AS weekday, Time, COUNT(*) AS sales_count 
FROM walmartsalesdata 
GROUP BY weekday, Time 
ORDER BY weekday;

#2. Which customer type brings the most revenue?

SELECT `Customer type` , SUM(total) AS total_revenue 
FROM walmartsalesdata 
GROUP BY `Customer type`
ORDER BY total_revenue DESC 
LIMIT 1;

#3. Which city has the largest tax percentage (VAT)?

SELECT city, AVG(`Tax 5%`) AS avg_tax 
FROM walmartsalesdata 
GROUP BY city 
ORDER BY avg_tax DESC 
LIMIT 1;

#4. Which customer type pays the most in VAT?

SELECT `Customer type`, SUM(`Tax 5%`) AS total_vat 
FROM walmartsalesdata 
GROUP BY `Customer type` 
ORDER BY total_vat DESC 
LIMIT 1;


#Customer Analysis
#1. How many unique customer types does the data have?

SELECT customer_type, COUNT(*) AS count FROM sales GROUP BY customer_type;

#2. How many unique payment methods does the data have?

SELECT payment, COUNT(*) AS count FROM sales GROUP BY payment;

#3. What is the most common customer type?

SELECT customer_type, COUNT(*) AS count 
FROM sales 
GROUP BY customer_type 
ORDER BY count DESC 
LIMIT 1;

#4. Which customer type buys the most?

SELECT customer_type, SUM(quantity) AS total_bought 
FROM sales 
GROUP BY customer_type 
ORDER BY total_bought DESC 
LIMIT 1;

#5. What is the gender of most of the customers?

SELECT gender, COUNT(*) AS count 
FROM sales 
GROUP BY gender 
ORDER BY count DESC 
LIMIT 1;

#6. What is the gender distribution per branch?

SELECT branch, gender, COUNT(*) AS count 
FROM sales 
GROUP BY branch, gender 
ORDER BY branch, count DESC;

#7. Which time of the day do customers give most ratings?

SELECT time_of_day, AVG(rating) AS avg_rating 
FROM sales 
GROUP BY time_of_day 
ORDER BY avg_rating DESC 
LIMIT 1;

#8. Which time of the day do customers give most ratings per branch?

SELECT branch, time_of_day, AVG(rating) AS avg_rating 
FROM sales 
GROUP BY branch, time_of_day 
ORDER BY branch, avg_rating DESC;

#9. Which day of the week has the best average ratings?

SELECT DAYNAME(date) AS weekday, AVG(rating) AS avg_rating 
FROM sales 
GROUP BY weekday 
ORDER BY avg_rating DESC 
LIMIT 1;

#10. Which day of the week has the best average ratings per branch?

SELECT branch, DAYNAME(date) AS weekday, AVG(rating) AS avg_rating 
FROM sales 
GROUP BY branch, weekday 
ORDER BY branch, avg_rating DESC;
