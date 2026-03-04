# Aggregate functions

Aggregate functions allow you to do calculations on the data in the table to return a single value. 

## MIN()
[Game Play Analysis I](https://leetcode.com/problems/game-play-analysis-i/description/)
select player_id, min(event_date) as first_login
from Activity
group by player_id

Aggregate functiions:
count:
## MAX()
https://leetcode.com/problems/the-latest-login-in-2020/

## COUNT()
https://leetcode.com/problems/customer-placing-the-largest-number-of-orders/description/
https://leetcode.com/problems/biggest-single-number/

## SUM()

## AVG()
AVG() can be used to find the average value across a column in a table

[Average Selling Price](https://leetcode.com/problems/average-selling-price/description/)
<chapter title="Answer" collapsible="true">
    <code-block lang="sql"> select t.product_id, round(cast(sum(t.money) as float) / sum(t.units),2) AS average_price from
(
    select p.product_id, u.units as units, u.units * p.price as money   from unitssold as u
    join prices as p on 
    u.product_id = p.product_id 
    and p.start_Date <= u.purchase_date 
    and p.end_date >= u.purchase_date
) as t
group by t.product_id </code-block>
    Explanation:  

The subquery in this lets us find what the price of the UnitsSold items was, which we can get from knowing which date range
the items were sold in. Once we know this we then return what product id was sold, how many units and units * price 
to get the total amount of money that was recieved for the items. 
<code-block lang="sql"> 
    select p.product_id, u.units as units, u.units * p.price as money   from unitssold as u
    join prices as p on 
    u.product_id = p.product_id 
    and p.start_Date <= u.purchase_date 
    and p.end_date >= u.purchase_date 
group by t.product_id </code-block>

We then use the result of this sub query to return the product id, and get the sum of money got for that product over time
divided by how many units we have sold over time.
<code-block lang="sql"> select t.product_id, sum(t.money) / sum(t.units)  AS average_price from
(
    [subquery] 
group by t.product_id </code-block>

The most annoying part of this is getting it down to two decimal places. we have to cast money as a float otherwise it will
return average_price as an integer. Then we can round the results to two decimal places.
<code-block lang="sql"> round(cast(sum(t.money) as float) / sum(t.units),2) AS average_price </code-block>

| SQL                             | What it does                                                                                                                                   | 
|---------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------| 
| SELECT product_id FROM Products | Because we are told we only want the product_id returning as output, here we specify which column we want returning                            | 
| WHERE low_fats = 'Y'            | The ENUM Category has two values 'Y' for yes and 'N' for no. Because we only want low_fat products, we only want Products where low_fats is Y. | 
| AND recyclable = 'Y'            | Because we only want recyclable products, we only want Products where recyclable is Y                                                          |
</chapter>
https://leetcode.com/problems/average-selling-price/
select t.product_id, round(cast(sum(t.money) as float) / sum(t.units),2) AS average_price from
(
    select p.product_id, u.units as units, u.units * p.price as money   from unitssold as u
    join prices as p on
    u.product_id = p.product_id
    and p.start_Date <= u.purchase_date
    and p.end_date >= u.purchase_date
) as t
group by t.product_id
## min 




