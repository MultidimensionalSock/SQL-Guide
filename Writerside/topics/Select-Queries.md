# Select Queries

Select queries allow you to view data from within the database, you can specify conditions on the data 
that will be returned so you can get only data you need. 

A simple select query will have the following: 
<code-block lang="sql"> SELECT [rows you want returning] 
FROM [Database table you want the data from] 
WHERE [condition] </code-block>

For the examples on this page I will be using a database table with the following Columns as examples:

| ColumnName  | Data Type | 
|-------------|-----------| 
| UserID      | int       | 
| FirstName   | varchar   | 
| LastName    | varchar   | 
| DateOfBirth | datetime  | 
| Country     | varchar   | 
| DateJoined  | datetime  | 

## Defining which rows you want returning 
If you want to only return certain data from the table. e.g. you want to only return the date of births from a user,
you can specify this by putting the name of the column after SELECT.
<code-block lang="sql"> SELECT DateOfBirth
FROM Users </code-block>

If you want to return more than one column then you can add more columns by adding the names broken up by commas
e.g. You want to return their Date of Birth AND the users first and last name
<code-block lang="sql"> SELECT DateOfBirth, FirstName, LastName
FROM Users </code-block>

A lot of the time, you might just want all the data in that row to be returned, instead of having to write out the name of 
each column in the table, you can use Wildcard to just get all rows: 
<code-block lang="sql"> SELECT * FROM Users </code-block>

## WHERE conditions 
Database tables can contain millions of records in them, returning all of these and then trying to find which record you're 
after is slow for the database, and slow for you. Using WHERE conditions allows you to filter the search down to only the data you want

### Where value equals 
If you're looking for only records where one column equals a specific value then you can use a statement like this: 
<code-block lang="sql"> SELECT * FROM Users
Where FirstName = 'John' </code-block>
When you run this, it will return the data for every user you have in your database table where someone is called john. 

Maybe you're looking for people who are called 'John Smith' though, you'll need to add another condition so it filters 
for both 
<code-block lang="sql"> SELECT * FROM Users
WHERE FirstName = 'John'
AND LastName = 'Smith'</code-block>
Now every user where their name is John Smith will be returned.

Or maybe you want it to be where the users first name is John OR their last name is Smith: 
<code-block lang="sql"> SELECT * FROM Users
WHERE FirstName = 'John'
OR LastName = 'Smith'</code-block>

If you're looking for one specific guy named 'John Smith' though, in a big database this could return hundreds of John Smiths, 
database usually have a 'Primary Key', this is a value which unique for each record in the table. For this user table that 
would be 'UserID', each user has a unique UserID to identify them, and if you know a users UserID, you can use this to find 
just their record 
<code-block lang="sql"> SELECT * FROM Users
WHERE UserID = 12345 </code-block>

Practice Tasks:

[Recylable and low fat products](https://leetcode.com/problems/recyclable-and-low-fat-products/description/ )
<chapter title="Answer" collapsible="true">
    <code-block lang="sql"> SELECT product_id FROM Products 
WHERE low_fats = 'Y'
AND recyclable = 'Y' </code-block>
    Explanation:  

| SQL                             | What it does                                                                                                                                   | 
|---------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------| 
| SELECT product_id FROM Products | Because we are told we only want the product_id returning as output, here we specify which column we want returning                            | 
| WHERE low_fats = 'Y'            | The ENUM Category has two values 'Y' for yes and 'N' for no. Because we only want low_fat products, we only want Products where low_fats is Y. | 
| AND recyclable = 'Y'            | Because we only want recyclable products, we only want Products where recyclable is Y                                                          |
</chapter>

[Not Boring Movies](https://leetcode.com/problems/not-boring-movies/description/ )
<chapter title="Answer" collapsible="true">
    <code-block lang="sql"> select * from Cinema
where id % 2 = 1
and description != 'boring'
order by rating desc </code-block>
    Explanation:  

| SQL                         | What it does                                                                                                                                  | 
|-----------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------| 
| select * from Cinema        | The output columns we need are the same as the input columns, so we can use '*'                                                               | 
| where id % 2 = 1            | to find out if id is odd we can use modulo to divide by 2 and return the remainder, if it returns anything other than 0, then we know its odd | 
| and description != 'boring' | Checks that the description is not 'boring' and removes any records where the movie is boring                                                 |
| order by rating desc        | Because we need to return 'ordered by rating in descending order.' we can use an order by to sort the records from highest to lowest          |
</chapter>
 
[Article Views I](https://leetcode.com/problems/article-views-i/)
<chapter title="Answer" collapsible="true">
    <code-block lang="sql"> SELECT distinct author_id AS id
FROM views 
WHERE author_id = viewer_id </code-block>
    Explanation:  

| SQL                                        | What it does                                                                                                                                                                                                                                       | 
|--------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------| 
| SELECT distinct author_id AS id FROM views | We only want to return the authors ID once, so we can use 'distinct' to say if there's more than one record, we only want the id returning once. as 'id' is used to change the name of the returned column to match the output wanted by the task. | 
| WHERE author_id = viewer_id                | We use this to find records where the author is viewing their own article as in that case they should be equal                                                                                                                                     | 
</chapter>

### Where value like 
Sometimes you might want data where you don't know the exact value of the entire column, or the value changes 
each time, but part of it stays the same. With the user table this is less likely to happen but if you're 
searching through more complex database tables e.g. a record of emails. You might be looking for a specific phrase in 
a column, in that case, LIKE can be really useful for finding rows which contain a value. 

For this example, say you want to return every user whose name starts with 'J' you can do this: 
<code-block lang="sql"> SELECT * FROM Users
WHERE FirstName LIKE 'J%' </code-block>

% is used after J to show that the column just needs to start with J and any value after that isn't important to 
whether or not the row should be returned on not. Where the '%' shows up in your like statement changes how it behaves.
e.g. if you want users whose names end with J rather than start with J, you can put it before the J:
<code-block lang="sql"> SELECT * FROM Users
WHERE FirstName LIKE '%J' </code-block>

or if you want names which have a J in them but it doesn't matter where in the data this J is:
<code-block lang="sql"> SELECT * FROM Users
WHERE FirstName LIKE '%J%' </code-block>

Practice Task:

NOTE: this requires you also know case statements

[DNA Pattern Recognition]( https://leetcode.com/problems/dna-pattern-recognition/)
<chapter title="Answer" collapsible="true">
    <code-block lang="sql"> select sample_id, dna_sequence, species,
    case when dna_sequence like 'ATG%' then 1 else 0 end as has_start,
    case 
        when dna_sequence like '%TAA' 
        OR dna_sequence like '%TAG' 
        OR dna_sequence like '%TGA' then 1 
        else 0 
        END AS has_stop,
    case when dna_sequence like '%ATAT%' then 1 else 0 end as has_atat,
    case when dna_sequence like '%GGG%' then 1 else 0 end as has_ggg
from Samples </code-block>
    Explanation:  

| SQL                                                                                                      | What it does                                                                                                                                                                                       | 
|----------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------| 
| select sample_id, dna_sequence, species,                                                                 | return values from base table which is needed in the output                                                                                                                                        | 
| case when dna_sequence like 'ATG%' then 1 else 0 end as has_start,                                       | This allows us to specify a value to be returned it a column is true and return it as its own column. ATG% will return if the dna_sequence starts with ATG and we return 1 if it does and 0 if not | 
| case when dna_sequence like '%TAA' OR dna_sequence like '%TAG' OR dna_sequence like '%TGA' then 1 else 0 | condition to satisfy Sequences that end with either TAA, TAG, or TGA                                                                                                                               |
| case when dna_sequence like '%ATAT%' then 1 else 0 end as has_atat,                                      | condition to satisfy Sequences containing the motif ATAT                                                                                                                                           |
| case when dna_sequence like '%GGG%' then 1 else 0 end as has_ggg                                         | condition to satify Sequences that have at least 3 consecutive G (like GGG or GGGG)                                                                                                                |
</chapter>

### Where less or more then 
When dealing with numbers in sql you can use standard numerical operators to compare values. 

Practice Tasks:

[Invalid Tweets](https://leetcode.com/problems/invalid-tweets/)
<chapter title="Answer" collapsible="true">
    <code-block lang="sql"> SELECT tweet_id FROM Tweets 
WHERE LEN(content) > 15  </code-block>
    Explanation:  

| SQL                         | What it does                                                                                   | 
|-----------------------------|------------------------------------------------------------------------------------------------| 
| SELECT tweet_id FROM Tweets  | specifies that we only want tweet_id returning                                                 | 
| WHERE LEN(content) > 15     | LEN() lets us found how many charracters are in text, if a tweet has more than 15 its invalid. |  
</chapter>

## Value in list
Sometimes you might want to return records where a value could equal one of multiple things. Instead of
doing a bunch of OR statements for the same column it can sometimes be easier to check if the value is in a list:
<code-block lang="sql"> SELECT * FROM Users
WHERE Country IN ('United Kingdom','Ireland','Spain')  </code-block>
This will return all data from Users where the user comes from the United Kingdom, Ireland or Spain

## Values in time frames
Sometimes you might want data within a range. E.g. every user who was born in January. Time is a slightly more complex 
example than other data types as if we only want users born in January, we also need to ignore the year they were born. 
You can get specific data from a date using: 
- DAY() - gets only the day from a datetime
- MONTH() - gets only the month from a datetime 
- YEAR() - gets only the year from a datetime 
In this case, we need month 
<code-block lang="sql"> SELECT * FROM Users
WHERE MONTH(DateOfBirth) = 1 </code-block>

If we care about which year then we could use the entire date and do the range between two time frames and not break it
down at all. E.g. if we want all users born in January where they were also born in 2002.
<code-block lang="sql"> SELECT * FROM Users
WHERE DateOfBirth >= '2002-01-01' and DateOfBirth <= '2002-01-31'  </code-block>

A more fun one is finding all uses who had their account made in the last week, then we can compare from the current time using 
GETDATE() 
<code-block lang="sql"> SELECT * FROM Users
WHERE DateJoined > DATEADD(week, -1, GETDATE())  </code-block>

The DATEADD() functions requires 3 values: DATEADD([time type], [amount to add or reduce], [datetime to add or reduce from])
The type of time can be one of the following

| Time Type   | SQL Examples                                                                                                              | 
|-------------|---------------------------------------------------------------------------------------------------------------------------| 
| millisecond | <code-block lang="sql"> SELECT * FROM Users WHERE DateJoined > DATEADD(millisecond, -604800000, GETDATE())  </code-block> |
| second      | <code-block lang="sql"> SELECT * FROM Users WHERE DateJoined > DATEADD(second, -604800, GETDATE())  </code-block>         |
| minute      | <code-block lang="sql"> SELECT * FROM Users WHERE DateJoined > DATEADD(minute, -10080, GETDATE())  </code-block>          |
| hour        | <code-block lang="sql"> SELECT * FROM Users WHERE DateJoined > DATEADD(hour, -168, GETDATE())  </code-block>              |
| day         | <code-block lang="sql"> SELECT * FROM Users WHERE DateJoined > DATEADD(day, 7, GETDATE())  </code-block>                  |
| weekday     | <code-block lang="sql"> SELECT * FROM Users WHERE DateJoined > DATEADD(weekday, -5, GETDATE())  </code-block>             |
| week        | <code-block lang="sql"> SELECT * FROM Users WHERE DateJoined > DATEADD(week, -1, GETDATE())  </code-block>                |
| month       | <code-block lang="sql"> SELECT * FROM Users WHERE DateJoined > DATEADD(month, -1, GETDATE())  </code-block>               |
| quarter     | <code-block lang="sql"> SELECT * FROM Users WHERE DateJoined > DATEADD(quarter, -1, GETDATE())  </code-block>             |
| year        | <code-block lang="sql"> SELECT * FROM Users WHERE DateJoined > DATEADD(year, -1, GETDATE())  </code-block>                |

## Select from another query 
Within SQL you can nest queries as long as the nested query is within brackets. Nesting queries can sometimes be used to
avoid joins, most nesting can be solved by joins, but it may be easier to just nest. One thing to be noted is that if 
you're using 'WITH (NOLOCK)' with a query to stop the table being locked, it will need to be used on the main and sub 
query if you want both to not lock tables. 

<code-block lang="sql"> SELECT * FROM Users
WHERE UserID in (SELECT UserID from Users where Country = 'United Kingdom' </code-block>
This is not the best example of how to use it, in this case we could just use a WHERE statement but if the users country
was stored in another table it might be a better use case, if we didn't want to join any tables. 

Practice Tasks:

[Primary Department for Each Employee](https://leetcode.com/problems/primary-department-for-each-employee/description/)
<chapter title="Answer" collapsible="true">
    <code-block lang="sql"> select employee_id, department_id 
from Employee
where primary_flag = 'Y'
OR employee_id in
(select employee_id from Employee 
group by employee_id 
having count(*) < 2)  </code-block>
    Explanation:  

| SQL                                             | What it does                                                                                                                                                                  | 
|-------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------| 
| select employee_id, department_id from Employee | specifies which columns we want returning                                                                                                                                     | 
| where primary_flag = 'Y'                        | If the user has a primary flag of Y we know this must be their primary department already so can instantly return that                                                        |
| OR employee_id in                               | This OR will allow us to find employees that are within the list of IDs returned by the subquery                                                                              | 
| select employee_id from Employee                | We only want to return the employee_id from this query as if there is more returned it can't be used as a list of employee_id's to compare against in the list                | 
| group by employee_id                            | this will make it so all records where the employee_id shows up are now in a group and considered as the same object for return values                                        | 
| having count(*) < 2                             | because we know if an employee has more than 1 department they will have a Y flag for primary, we only need to get the employee IDs where they only show up in one department | 
</chapter>


## Glossary 
| SQL snippet   | What it does                                                                                                                                                                                                                                                                                                                            | SQL Example                                                                         | 
|---------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:------------------------------------------------------------------------------------| 
| * (Wildcard)  | Is used to select all rows from the table which fit your conditions                                                                                                                                                                                                                                                                     | <code-block lang="sql"> SELECT * FROM Users;</code-block>                           |
| TOP           | Is used to select how many rows you want returning from the query at maximum                                                                                                                                                                                                                                                            | <code-block lang="sql"> SELECT TOP 1 * FROM Users;</code-block>                     |
| WITH (NOLOCK) | When doing queries, it is standard that the database table is locked until the query is finished, so no other operations can be run on it at the same time. For SELECT statements you can specify that you don't want the table to be locked, which will allow others to access or edit the table at the same time the table is running | <code-block lang="sql">SELECT * FROM Users WITH (NOLOCK) </code-block>              |
| % (LIKE)      | Used to specify where data can change after/before this point but still be a valid return                                                                                                                                                                                                                                               | <code-block lang="sql"> SELECT * FROM Users WHERE FirstName LIKE 'J%' </code-block> |                                                                                                                                                                                                                                                                                                                                         | 
