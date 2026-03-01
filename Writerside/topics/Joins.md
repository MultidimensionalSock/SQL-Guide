# Joins
SQL joins allow you to get data from two tables and collapse them into one record, this allows you to return data from 
two or more tables within one query.

To combine two tables you need a value (or multiple values when combined) which match in both tables. 
The value from your first table (Table A) will usually be your primary key, a value which is unique to that record,
and the value in the second table (Table B) will be a foreign key, a value which matches the primary key or the first table
but may or may not be unique to the record in Table B. To specify the value where the join should happen we use 'ON'

e.g.
<code-block lang="sql"> JOIN [Table B] on [TableA.PrimaryKey] = [TableB.ForeignKey] </code-block>

Tables which will be used as examples;

UserBalance

| ColumnName  | Data Type | 
|-------------|-----------| 
| UserID      | int       | 
| FirstName   | varchar   | 
| LastName    | varchar   | 
| DateOfBirth | datetime  | 
| Country     | varchar   | 
| DateJoined  | datetime  | 

Deposits

| ColumnName | Data Type |
|------------|-----------|
| DepositID  | int       |
| UserID     | int       |
| Amount     | float     |
| DateTime   | datetime  | 

SQL has four main types of join: 

## JOIN 
Also known as INNER JOIN. JOIN allows you to return rows which have matching values in both tables. 
If there isnt a matching value in both tables (e.g. if a user has made no deposits) then no data will be returned from 
either table
![join.png](join_2.png)

Example - Return all deposit IDs for deposits made by users who joined in the last day
<code-block lang="sql"> SELECT d.DepositID FROM Deposits as d 
JOIN Users as u on d.userid = u.userid 
where u.DateJoined > DATEADD(day, -1, GETDATE()) </code-block>
as you can see above, I shortened the names of the database tables down to single letter identifiers, 
this is useful when dealing with a long database names or where you with have a lot of returns and we don't want all rows returning. 
Instead of having to write out the whole table name to say which table the value should come from e.g. Users.userid, we can instead 
just write u.userid. 

You don't need to use 'AS' to rename the database table for the query, you can just put the new identifier after the 
database name, but it helps make it clearer. e.g. Without AS 
<code-block lang="sql"> SELECT d.DepositID FROM Deposits d
JOIN Users u on d.userid = u.userid 
where u.DateJoined > DATEADD(day, -1, GETDATE()) </code-block>

Practice Tasks:
[Combine Two Tables](https://leetcode.com/problems/combine-two-tables/description/)
<chapter title="Answer" collapsible="true">
    <code-block lang="sql"> select p.firstName, p.lastName, a.city, a.state 
from Person as p
join Address as a on p.personId = a.personId </code-block>
    Explanation:  

| SQL                                             | What it does                                                                                                            | 
|-------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------| 
| select p.firstName, p.lastName, a.city, a.state | Specifies which columns of a record we want returning, with which database table the value should come from             | 
| from Person as p                                | FROM statement for our first table as well as the new identifier                                                        | 
| join Address as a on p.personId = a.personId    | this joins the address and users table, the common value between both tables is the personId so this is what we join on |
</chapter> 

[Product Sales Analysis i](https://leetcode.com/problems/product-sales-analysis-i/)
<chapter title="Answer" collapsible="true">
    <code-block lang="sql"> select p.product_name, s.year, s.price
from Product as p
join Sales as s on p.product_id = s.product_id </code-block>
    Explanation:  

| SQL                                            | What it does                                                                                                | 
|------------------------------------------------|-------------------------------------------------------------------------------------------------------------| 
| select p.product_name, s.year, s.price         | Specifies which columns of a record we want returning, with which database table the value should come from | 
| from Product as p                              | FROM statement for our first table as well as the new identifier                                            | 
| join Sales as s on p.product_id = s.product_id | this joins the two tables on the value which matches between both                                           |
</chapter> 


## LEFT JOIN 
Also known as LEFT OUTER JOIN. This type of join will return ALL records from Table A and records from Table B if a match is found. 
If no match is found, any column from Table B being returned will show the values as NULL.
![left outer join.png](left outer join.png) 

Practice Tasks:
[Replace Employee ID with the unique Identifier](https://leetcode.com/problems/employee-bonus/description/)
<chapter title="Answer" collapsible="true">
    <code-block lang="sql"> SELECT e.name, b.Bonus from Employee as e
left join Bonus as b on e.empId = b.empId
where b.empid is null
or b.bonus < 1000 </code-block>
    Explanation:  

| SQL                                       | What it does                                                                                                             | 
|-------------------------------------------|--------------------------------------------------------------------------------------------------------------------------| 
| SELECT e.name, b.Bonus from Employee as e | Specifies which columns of a record we want returning, with which database table the value should come from              | 
| left join Bonus as b on e.empId = b.empId | join the two tables on a value which matches between both                                                                | 
| where b.empid is null                     | if empid is null in the Bonus table, that means there's no record of the employee getting a bonus                        |
| or b.bonus < 1000                         | condition check that if the employee has a record in the bonus table but got less than 1000, then they are also returned |
</chapter> 

[Employee Bonus](https://leetcode.com/problems/replace-employee-id-with-the-unique-identifier/)
<chapter title="Answer" collapsible="true">
    <code-block lang="sql"> select uni.unique_id, e.name
from Employees as e
left join EmployeeUNI as uni on e.id = uni.id </code-block>
    Explanation:  

| SQL                                           | What it does                                                                                                | 
|-----------------------------------------------|-------------------------------------------------------------------------------------------------------------| 
| select uni.unique_id, e.name                  | Specifies which columns of a record we want returning, with which database table the value should come from | 
| from Employees as e                           | FROM statement for our first table as well as the new identifier                                            | 
| left join EmployeeUNI as uni on e.id = uni.id | we are using left join here because we want all employees to be returned even if they don't have a uniqueid |
</chapter> 
 
## RIGHT JOIN 
Also known as RIGHT OUTER JOIN. This type of join will return ALL records from Table B and records from Table A if a match is found.
If no match is found, any column from Table A being returned will show the values as NULL.
![right.png](right join.png)

## FULL JOIN
Also known as FULL OUTER JOIN. This type of join returns all the records from Table A and Table B, even if no match is found.
Where a match is found then it will combine the two reords, where its not the values from the unmatched table will be shown 
as NULL.
![full join.png](full join.png)
 








