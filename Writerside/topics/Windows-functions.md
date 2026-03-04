# Windows functions

https://leetcode.com/problems/rank-scores/
select score, dense_rank()
over (order by score desc) as rank
from Scores 

https://leetcode.com/problems/department-highest-salary/description/
select
d.name as Department,
e.Name as Employee,
e.salary from employee as e
join (select id, name, max(salary)
over (partition by departmentid) as maxi
from employee) as e2
on e.id = e2.id
join Department as d on e.departmentId  = d.id
and e.salary = e2.maxi

RANK
https://leetcode.com/problems/rank-scores/
https://leetcode.com/problems/department-highest-salary/