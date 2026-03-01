# Group by

https://leetcode.com/problems/group-sold-products-by-the-date/description/

## Distinct values
https://leetcode.com/problems/number-of-unique-subjects-taught-by-each-teacher/


https://leetcode.com/problems/bank-account-summary-ii/
select u.name, sum(t.amount) as balance
from Transactions as t
join Users as u on u.account = t.account
group by u.account, u.name
having sum(t.amount) > 10000   