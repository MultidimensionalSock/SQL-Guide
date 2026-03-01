# Updates

https://leetcode.com/problems/swap-sex-of-employees/description/ 
Update Salary
Set sex =
Case when sex = 'm' then 'f'
when sex = 'f' then 'm'
end;

https://leetcode.com/problems/find-total-time-spent-by-each-employee/
Note: this was done with MySQL as T-SQL is a pain about this due to groups having to contain all returned values...
select event_day as day ,
emp_id, sum(out_time - in_time) as total_time
from Employees
Group by emp_id, event_day