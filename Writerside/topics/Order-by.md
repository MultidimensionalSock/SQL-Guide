# Order by
Order by is used at the end of an sql statement
it allows you to order a database table by ascending (asc) or decending (desc) based on column(s) you pass to it. It can sort numerically and alphabetically.

If you don't specify whether it should order by ASC or DESC it will automatically order by ASC.

For the rest of the examples here the following database table will be used

Deposits

| Column Name   | Data Type |
| ------------- | --------- |
| DepositID     | INT       |
| UserID        | INT       |
| Amount        | FLOAT     |
| Date          | DateTime  |
| PaymentMethod | VARCHAR   |

With the following details for examples:

| DepositID | UserID | Amount | Date       | PaymentMethod |
| --------- | ------ | ------ | ---------- | ------------- |
| 204342    | 124211 | 10.00  | 03/03/2026 | Debit Card    |
| 204345    | 124211 | 10.00  | 03/03/2025 | Debit Card    |
| 105332    | 342421 | 11.11  | 02/03/2025 | Paypal        |
| 202422    | 352432 | 3.00   | 01/03/2026 | Debit Card    |
| 202423    | 342421 | 14.11  | 02/03/2025 | Credit Card   |

## Order by ascending
Ordering by Ascending orders all the records you're selecting from smallest to biggest. ASC is the default type of order in SQL so you don't have to add 'ASC' explicitly, but it can help to make your SQL clearer.

e.g. If you want to order all the Deposits by Amount you could do:
<code-block lang="sql"> SELECT * FROM Deposits ORDER BY Amount
(or SELECT * FROM Deposits ORDER BY Amount ASC) </code-block>


When you run this is will return:

| DepositID | UserID | Amount | Date       | PaymentMethod |
| --------- | ------ | ------ | ---------- | ------------- |
| 202422    | 352432 | 3.00   | 01/03/2026 | Debit Card    |
| 204342    | 124211 | 10.00  | 03/03/2026 | Debit Card    |
| 204345    | 124211 | 10.00  | 03/03/2025 | Debit Card    |
| 105332    | 342421 | 11.11  | 02/03/2025 | Paypal        |
| 202423    | 342421 | 14.11  | 02/03/2025 | Credit Card   |

## Order by descending
Ordering by Descending orders records from biggest to smallest. To make an 'order by' statement work in descending order, you need to put DESC after the field/s you're ordering by.

e.g. if you want to order by which deposit has happened most recently:
<code-block lang="sql"> SELECT * FROM Deposits ORDER BY Date DESC </code-block>


This would return:

| DepositID | UserID | Amount | Date       | PaymentMethod |
| --------- | ------ | ------ | ---------- | ------------- |
| 105332    | 342421 | 11.11  | 02/03/2025 | Paypal        |
| 202423    | 342421 | 14.11  | 02/03/2025 | Credit Card   |
| 204345    | 124211 | 10.00  | 03/03/2025 | Debit Card    |
| 202422    | 352432 | 3.00   | 01/03/2026 | Debit Card    |  
| 204342    | 124211 | 10.00  | 03/03/2026 | Debit Card    |


## Ordering by serveral columns
Order bys let you order by more than one column at a time, it will order by the first column you name first, then the second, etc.

e.g. If you want to order by UserID, which will make all of the deposits with matching users IDs appear next to each other, and then by Date so you can see all of a users deposits from newest to oldest, You could do:
<code-block lang="sql"> SELECT * FROM Deposits
ORDER BY USERID, Date DESC </code-block>

This will return:

| DepositID | UserID | Amount | Date       | PaymentMethod |
| --------- | ------ | ------ | ---------- | ------------- |
| 202422    | 352432 | 3.00   | 01/03/2026 | Debit Card    |  
| 105332    | 342421 | 11.11  | 02/03/2025 | Paypal        |
| 202423    | 342421 | 14.11  | 02/03/2025 | Credit Card   |
| 204342    | 124211 | 10.00  | 03/03/2026 | Debit Card    |
| 204345    | 124211 | 10.00  | 03/03/2025 | Debit Card    |

This makes it so not just the date is from latest to oldest, but that the UserIds also go from biggest to smallest.

But maybe we instead want the smallest userIds first, and then the deposits ordered newest to oldest. Order by's allow you to specifify ASC or DESC for specific Columns.
e.g. 
<code-block lang="sql"> SELECT * FROM Deposits
ORDER BY USERID ASC, Date DESC </code-block>

this now returns:

| DepositID | UserID | Amount | Date       | PaymentMethod |
| --------- | ------ | ------ | ---------- | ------------- |
| 204342    | 124211 | 10.00  | 03/03/2026 | Debit Card    |
| 204345    | 124211 | 10.00  | 03/03/2025 | Debit Card    |
| 202422    | 352432 | 3.00   | 01/03/2026 | Debit Card    |  
| 105332    | 342421 | 11.11  | 02/03/2025 | Paypal        |
| 202423    | 342421 | 14.11  | 02/03/2025 | Credit Card   |

NOTE: If you're doing a join with an order by, JOINs will happen before the records are ordered
