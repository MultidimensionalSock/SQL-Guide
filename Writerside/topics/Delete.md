# Delete

DELETE statements let you remove data from a table where the data has a certain condition - or no condition if you
just want to delete all the records in a table. 

They follow this syntax: 
<code-block lang="sql"> DELETE FROM [Table_Name] WHERE [Condition] </code-block>

Practice Questions: 

[Delete Duplicate Tables](https://leetcode.com/problems/delete-duplicate-emails/)
<chapter title="Answer" collapsible="true">
    <code-block lang="sql"> delete from Person where id not in
(
select min(id) from Person
group by email
) </code-block>
    Explanation:  

| SQL                                        | What it does                                                                                                                                                                                   | 
|--------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------| 
| delete from Person where id not in         | Deletes all records from the person table where the ID is not in the list of IDs returned by the subquery (explained next)|
| SELECT MIN(id) FROM Person group by email  | This will select the smallest ID for each email group, if there is only one email it will be the only id, if there's more than one it will return the record in the group with the smallest ID | 
</chapter> 

