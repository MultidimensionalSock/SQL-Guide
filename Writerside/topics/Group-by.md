# Group by

Group by allows you to create groups of data that share similar data, you can then do calculations across a group,
rather than every record in the table. When grouping data only one record can be returned 
for each group, you can only return the values which the records are being grouped by, and values which are aggregates
of all of the records in the group 

Examples of how groups can be used with aggregates:
- COUNT() - Can be used to find how many records are in each group 
- SUM() - Find the sum of a column when all the records value for that column is added
- AVG() - Find the average value of a column across all records in the group 
- MAX() - Find the max value for a column in that group 
- MIN() - Find the minimum value for a column in that group

For the rest of the examples on this page we will be using this table: 

| Column Name | Data Type | 
| ----------- | --------- | 
| UserID      | INT       | 
| Name        | VARCHAR   |
| Department  | VARCHAR   |
| Salary      | INT       | 

With the following example data: 

Employees

| UserID | Name  | Department | Salary | 
| ------ |-------|------------| ------ |
| 10242  | Max   | IT         | 20000  |
| 10433  | Emma  | IT         | 25000  |
| 10453  | Jason | IT         | 25000  | 
| 10532  | Alice | HR         | 30000  | 
| 10634  | Robin | Retail     | 15000  |

## Grouping by one value
Grouping by one value will create groups where all of the records in that group share the same value in a column 

e.g. Finding how many employees work in each department 
<code-block lang="sql"> SELECT DEPARTMENT, COUNT(*) FROM Employees
GROUP BY Department </code-block> 

This will return:

| Department | Count | 
| ---------- |-------| 
| IT         | 3     | 
| HR         | 1     | 
| Retail     | 1     |

Practice Questions:


[Managers with at Least 5 Direct Reports](https://leetcode.com/problems/managers-with-at-least-5-direct-reports/description/)
<chapter title="Answer" collapsible="true">
    <code-block lang="sql"> SELECT Name from Employee 
where id in 
(
    Select managerId from Employee
    group by managerId 
    having count(*) > 4
) </code-block>
    Explanation:  

| SQL                                                                     | What it does                                                                                                                                         | 
|-------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------| 
| SELECT Name from Employee where id in                                   | Get the name of the manager for a return - this could also be done by joining the manager to their employee but in this case would be less efficient | 
| (Select managerId from Employee group by managerId having count(*) > 4) | Subquery which groups all employees by their managerID and then returns only managerIds where the group has 5 or more records                        | 
</chapter> 

[Classes With at Least 5 Students](https://leetcode.com/problems/classes-with-at-least-5-students/description/)
<chapter title="Answer" collapsible="true">
    <code-block lang="sql"> select class from Courses 
group by class 
having count(*) >= 5 </code-block>
 </chapter> 

## Grouping by two values 
You can also group by more than one value, when grouping by more than one it will group by the first value 
and then group that first group into smaller groups after that 

e.g. Finding how many employees in each department share the same wage 
<code-block lang="sql"> SELECT DEPARTMENT, SALARY COUNT(*) FROM Employees
GROUP BY Department, Salary </code-block> 

This will return:

| Department | Salary | Count |
| ---------- |------- | ----- |
| IT         | 20000  | 1     | 
| IT         | 25000  | 2     |
| HR         | 30000  | 1     | 
| Retail     | 15000  | 1     |

Practice Questions:
[Group Sold Products By The Date](https://leetcode.com/problems/group-sold-products-by-the-date/description/)
<chapter title="Answer" collapsible="true">
    <code-block lang="sql"> SELECT t.sell_date, COUNT(t.product) AS num_sold , string_agg(product, ',') AS products
FROM (
    SELECT sell_date, product 
    FROM Activities 
    GROUP BY sell_date, product
)   
GROUP BY t.sell_date </code-block>
    Explanation:  

| SQL                                                                                     | What it does                                                                                                                                                                                              | 
|-----------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------| 
| SELECT t.sell_date, COUNT(t.product) AS num_sold , string_agg(product, ',') AS products | Here we are returning the unique sell_date for the group, then we are getting the number of distinct products sold and using STRING_AGG to combine all of the product names sold that day into one string | 
| SELECT sell_date, product FROM Activities GROUP BY sell_date, product                   | This nested query allows us to get rid of any duplicates where the item was sold more than once in the same day                                                                                           | 
| GROUP BY t.sell_date                                                                    | We can then group the duplicate-filtered data again for each sell_date                                                                                                                                    |
</chapter> 

## HAVING
If you want to filter the results of a group, alongside being able to use 'WHERE' to reduce how many records need to be grrouped, 
we can use having. HAVING statements are used with aggregates whereas WHERE statements are used with conditions 

e.g. if you want to get the count of all Employees per department earning over 20000 you could do: 
<code-block lang="sql"> SELECT DEPARTMENT, COUNT(*) FROM Employees
WHERE salary > 20000
GROUP BY Department </code-block> 

This will return: 

| Department | Count | 
| ---------- |-------| 
| IT         | 3     | 
| HR         | 1     |  

Retail is excluded from the returned results because noone of the Employees working in Retail earn over 20000

A situation where you might use HAVING instead is where you want to return only departments where there is more than one 
employee
e.g. <code-block lang="sql"> SELECT DEPARTMENT FROM Employees
WHERE salary > 20000
GROUP BY Department
HAVING Count(*) > 1 </code-block>

Which will return:

| Department |
| ---------- |
| IT         |

Practice Questions:
[Duplicate Emails](https://leetcode.com/problems/duplicate-emails/)
<chapter title="Answer" collapsible="true">
    <code-block lang="sql"> select email from person
group by email having count(*) > 1 </code-block>
    Explanation:  

| SQL                                 | What it does                                                                                                                                                   | 
|-------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------| 
| group by email having count(*) > 1  | Group by email then if the group has more than one record in it, we know it has a duplicate record so we need to return it                                     | 
</chapter> 

[Biggest Single Number](https://leetcode.com/problems/biggest-single-number/)
<chapter title="Answer" collapsible="true">
    <code-block lang="sql"> SELECT TOP 1 num FROM MyNumbers
GROUP BY num  
HAVING COUNT(*) < 2 
ORDER BY num DESC </code-block>
    Explanation:  

| SQL                             | What it does                                                                                                                                                   | 
|---------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------| 
| SELECT TOP 1 num FROM MyNumbers | 'TOP 1' means only the highest number will be returned                                                                                                         | 
| GROUP BY num                    | grouping all the numbers means if there is any duplicates they will be grouped                                                                                 | 
| HAVING COUNT(*) < 2             | If a number is a duplicate, it can't be returned as its not the highest 'single' then, this allows us to remove groups which have more than one record in them |
| ORDER BY num DESC               | puts the highest number at the top so 'TOP 1' can return it |
</chapter> 

[Bank Account Summary II](https://leetcode.com/problems/bank-account-summary-ii/description/)
<chapter title="Answer" collapsible="true">
    <code-block lang="sql"> select u.name, sum(t.amount) as balance from Transactions as t
join Users as u on u.account = t.account
group by u.account, u.name
having sum(t.amount) > 10000  </code-block>
    Explanation:  

| SQL                                        | What it does                                                                                                                                                                                                                          | 
|--------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------| 
| join Users as u on u.account = t.account   | Joins users to their transactions so you can get their name for the output                                                                                                                                                            | 
| group by u.account, u.name                 | here we group account AND name because TSQL can only return plain values if they're in the group by, in this case we know the name will always be the same for all records but name is included in the group just for compile reasons | 
| having sum(t.amount) > 10000               | Filters out any groups where the users total balance is less than 10000                                                                                                                                                               |
</chapter>  