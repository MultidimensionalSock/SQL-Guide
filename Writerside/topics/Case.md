# Case

https://leetcode.com/problems/triangle-judgement/description/
select *,
case when (x + y) > z
and (y + z) > x
and (x + z) > y
then 'Yes'
else 'No'
end as triangle
from Triangle 