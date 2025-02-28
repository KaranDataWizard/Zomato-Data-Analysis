 **1 Which are the top 10 highest-rated restaurants?**

select name Restaurants,rate from zomato
group by name,rate
order by rate desc
limit 10
;
| Restaurant                 | Rate   |
|----------------------------|--------|
| Onesta                     | 4.6-5  |
| Meghana Foods              | 4.4-5  |
| Empire Restaurant          | 4.4-5  |
| Corner House Ice Cream     | 4.3-5  |
| Szechuan Dragon            | 4.2-5  |
| Smacznego                  | 4.2-5  |
| Cafe Shuffle               | 4.2-5  |
| The Coffee Shack           | 4.2-5  |
| Peppy Peppers              | 4.2-5  |
| Wamama                     | 4.2-5  |

**2 Which restaurants receive the most votes?**

select name Restaurants, votes from zomato
group by 1,2 
order by votes desc 
limit 1;

| Restaurant         | Votes |
|--------------------|-------|
| Empire Restaurant | 4884  |



**3 How many restaurants support both online orders and table booking?**

select  count(*) as Restaurants_Count from zomato
where online_order ="yes" and book_table = "yes";


| Count |
|-------|
| 7     |

**4 What percentage of restaurants support online orders?**

SELECT 
    ROUND(COUNT(CASE
                WHEN online_order = 'yes' THEN 1
            END) * 100 / COUNT(*),
            2) AS Online_order_percentage
FROM
    zomato;
  
| Online Order Percentage |
|-------------------------|
| 39.19%                  |


**5 Does table booking impact restaurant ratings?**

SELECT 
    book_table, ROUND(AVG(rate), 2)
FROM
    zomato
GROUP BY 1; 

| Book Table | Avg Rating |
|------------|-----------|
| Yes        | 4.19      |
| No         | 3.6       |

**6 What is the average cost for two people per restaurant type?**

SELECT 
    listed_in AS Restaurant_type,
    ROUND(AVG(approx_cost), 2) AS Avg_cost_for_two_people
FROM
    zomato
GROUP BY 1; 

| Restaurant Type | Avg Cost for Two |
|----------------|-----------------|
| Buffet         | 671.43           |
| Cafes          | 545.65           |
| Other          | 668.75           |
| Dining         | 357.27           |


**7	 Which are the 5 most expensive and 5 cheapest restaurants?**

(SELECT 
    name, approx_cost, 'Most_expensive' Category
FROM
    zomato
GROUP BY 1 , 2
ORDER BY 2 DESC
LIMIT 5) UNION ALL (SELECT 
    name, approx_cost, 'Cheapest' Category
FROM
    zomato
GROUP BY 1 , 2
ORDER BY 2 ASC
LIMIT 5);

| Name                        | Approx Cost | Category       |
|-----------------------------|------------|---------------|
| Ayda Persian Kitchen        | 950        | Most Expensive |
| K27 - The Pub               | 900        | Most Expensive |
| Cafe Coffee Day             | 900        | Most Expensive |
| Beijing Bites               | 850        | Most Expensive |
| Jeet Restaurant             | 850        | Most Expensive |
| Coffee Bytes                | 100        | Cheapest       |
| Melting Melodies            | 100        | Cheapest       |
| Namma Brahmin's Idli        | 100        | Cheapest       |
| Foodlieious Multi Cuisine   | 100        | Cheapest       |
| Chill Out                   | 100        | Cheapest       |


**8 What is the most common restaurant type?**

select listed_in Restaurant_type, count(*) Count from zomato
group by 1
order by 2 desc 
limit 1;

| Restaurant Type | Count |
|----------------|-------|
| Dining        | 110   |

**9 What is the average rating per restaurant type?**

select listed_in Restaurant_type, round(avg(rate),2) Avg_rating from zomato
group by 1 
order by 2 desc; 
select * from zomato;
 
| Restaurant Type | Avg Rating |
|----------------|-----------|
| Other         | 3.91      |
| Buffet        | 3.84      |
| Cafes         | 3.77      |
| Dining        | 3.57      |


**10 How does online ordering impact restaurant ratings?**

SELECT 
    online_order, ROUND(AVG(rate), 2) Rating
FROM
    zomato
WHERE
    online_order = 'yes'
GROUP BY 1
ORDER BY 2 DESC; 

| Online Order | Avg Rating |
|-------------|-----------|
| Yes         | 3.86      |


**11 How many restaurants fall into each price category (budget-friendly, mid-range, premium)?**

select 
case 
when approx_cost > 500 then 'Budget_friendly'
when approx_cost between 500 and 700 then "mid_range"
else "Premium" 
end as Price_Category, 
 count(*) Restaurant_count 
 from zomato 
 group by price_category
 order by 2 desc; 

| Price Category   | Restaurant Count |
|-----------------|-----------------|
| Premium        | 91              |
| Budget Friendly | 43              |
| Mid Range      | 14              |

**12 Which restaurant type has the highest number of restaurants?**

 SELECT 
    listed_in, COUNT(name) AS Restaurant_count
FROM
    zomato
GROUP BY 1
ORDER BY 2 DESC;

| Listed In | Restaurant Count |
|-----------|-----------------|
| Dining    | 110             |
| Cafes     | 23              |
| Other     | 8               |
| Buffet    | 7               |


**13 What is the average rating of restaurants that offer online orders vs. those that don’t?**

SELECT 
    online_order, ROUND(AVG(rate), 2)
FROM
    zomato
GROUP BY 1
;
 
| Online Order | Avg Rating |
|-------------|-----------|
| Yes         | 3.86      |
| No          | 3.49      |

**14 What is the average rating based on price category?**

SELECT 
    CASE
        WHEN approx_cost > 500 THEN 'Budget_friendly'
        WHEN approx_cost BETWEEN 500 AND 700 THEN 'mid_range'
        ELSE 'Premium'
    END AS Price_Category,
    ROUND(AVG(rate), 2) Avg_Rating
FROM
    zomato
GROUP BY price_category
ORDER BY 2 DESC; 

| Price Category   | Avg Rating |
|-----------------|-----------|
| Budget Friendly | 3.79      |
| Mid Range      | 3.71      |
| Premium        | 3.55      |

 
**15 How do ratings vary for restaurants with more than 500 votes?**
 
 select 
 case 
 when rate >=  4.5 then 'Excellent' 
 when rate between 4.0 and 4.49 then 'Good' 
 when rate between 3.5 and 3.99 then 'Average' 
 else "Below Average" 
 end as Rating_Category, 
 count(*) as Restaurant_Count 
from zomato 
where votes >=500 
group by Rating_Category
order by 2 desc;
  
| Rating Category | Restaurant Count |
|----------------|-----------------|
| Good          | 15              |
| Average       | 5               |
| Excellent     | 2               |
| Below Average | 1               |


