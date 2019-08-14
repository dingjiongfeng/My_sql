**【任务四】**#任务时间#
请于**8****月****15****日22:00前**完成，在[https://shimo.im/sheets/8tytcJVqY68tDjJv/z8q2D](https://shimo.im/sheets/8tytcJVqY68tDjJv/z8q2D)处打卡。逾期尚未打卡的会被清退。
## **4.1 MySQL 实战 - 复杂项目****#作业#**
### 项目十六 分数排名  （难度：中等）
依然是昨天的分数表，实现排名功能，但是排名需要是非连续的，如下：
+-------+------+
| Score | Rank |
+-------+------+
| 4.00  | 1    |
| 4.00  | 1    |
| 3.85  | 3    |
| 3.65  | 4    |
| 3.65  | 4    |
| 3.50  | 6    |
+-------+------

select s2.Score,(select count(score)+1 from Score as s1 where s1.Score>s2.Score) rank from Score as s2

order by s2.Score desc;

### 项目十七：查询回答率最高的问题 （难度：中等）
求出**survey_log**表中回答率最高的问题，表格的字段有：**uid, action, question_id, answer_id, q_num, timestamp**。
uid是用户id；action的值为：“show”， “answer”， “skip”；当action是"answer"时，answer_id不为空，
相反，当action是"show"和"skip"时为空（null）；q_num是问题的数字序号。
写一条sql语句找出回答率最高的问题。
**举例：**
**输入**
| 12345678 | +------+-----------+--------------+------------+-----------+------------+\| uid  \| action    \| question_id  \| answer_id  \| q_num     \| timestamp  \|+------+-----------+--------------+------------+-----------+------------+\| 5    \| show      \| 285          \| null       \| 1         \| 123        \|\| 5    \| answer    \| 285          \| 124124     \| 1         \| 124        \|\| 5    \| show      \| 369          \| null       \| 2         \| 125        \|\| 5    \| skip      \| 369          \| null       \| 2         \| 126        \|+------+-----------+--------------+------------+-----------+------------+ | 
|----|----|
**输出**
| 12345 | +-------------+\| survey_log  \|+-------------+\|    285      \|+-------------+ | 
|----|----|
**说明**
问题285的回答率为1/1，然而问题369的回答率是0/1，所以输出是285。
**注意：** 最高回答率的意思是：同一个问题出现的次数中回答的比例。

select question_id as survey_log from (select (sum(case when 'action' like 'answer' then 1 else 0 and) / sum(case when 'action' like 'show' then 1 else 0))

as rate,question_id from servey_log group by question_id order by rate desc limit 1);

### 项目十八：各部门前3高工资的员工（难度：中等）
将项目7中的employee表清空，重新插入以下数据（其实是多插入5,6两行）：
+----+-------+--------+--------------+
| Id | Name  | Salary | DepartmentId |
+----+-------+--------+--------------+
| 1  | Joe   | 70000  | 1            |
| 2  | Henry | 80000  | 2            |
| 3  | Sam   | 60000  | 2            |
| 4  | Max   | 90000  | 1            |
| 5  | Janet | 69000  | 1            |
| 6  | Randy | 85000  | 1            |
+----+-------+--------+--------------+

DELETE from employee;

insert into employee value(1,'Joe',70000,1);
insert into employee value(2,'Henry',80000,2);
insert into employee value(3,'Sam',60000,2);
insert into employee value(4,'Max',90000,1);
insert into employee value(5,'Janet',69000,1);
insert into employee value(6,'Randy',85000,1);

编写一个 SQL 查询，找出每个部门工资前三高的员工。例如，根据上述给定的表格，查询结果应返回：
+------------+----------+--------+
| Department | Employee | Salary |
+------------+----------+--------+
| IT         | Max      | 90000  |
| IT         | Randy    | 85000  |
| IT         | Joe      | 70000  |
| Sales      | Henry    | 80000  |
| Sales      | Sam      | 60000  |
+------------+----------+--------+

此外，请考虑实现各部门前N高工资的员工功能。

select (case when DeparmentId = 1 then 'IT' else 'Sales') Department,e.Name Employee,e.salary Salary from Employee e,Department d

where 3 > (select COUNT(DISTINCT e2.Salary) FROM Employee e2 WHERE e2.Salary > e1.Salary AND e1.DepartmentId = e2.DepartmentId)

group by DepartmentId order by e.Salary desc;

### 项目十九：平面上最近距离
**point_2d** 表包含一个平面内一些点（超过两个）的坐标值（x，y）。
写一条查询语句求出这些点中的最短距离并保留2位小数。
| x   | y   | 
|:----|:----|
| -1   | -1   | 
| 0   | 0   | 
| -1   | -2   | 

最短距离是1，从点（-1，-1）到点（-1，2）。所以输出结果为：
| shortest   | 
|:----|
| 1.00   | 

**注意：** 所有点的最大距离小于10000。

select round(SQRT(min(power(p1.x-p2.x,2)-power(p1.y-p2.y,2))),2) as shortest from point_2d p1 join point_2d p2

on p1.x!=p2.x and p1.y!=p2.y;

### 项目二十：行程和用户（难度：困难）
Trips 表中存所有出租车的行程信息。每段行程有唯一键 Id，Client_Id 和 Driver_Id 是 Users 表中 Users_Id 的外键。Status 是枚举类型，

枚举成员为 (‘completed’, ‘cancelled_by_driver’, ‘cancelled_by_client’)。

+----+-----------+-----------+---------+--------------------+----------+
| Id | Client_Id | Driver_Id | City_Id |        Status      |Request_at|
+----+-----------+-----------+---------+--------------------+----------+
| 1  |     1     |    10     |    1    |     completed      |2013-10-01|
| 2  |     2     |    11     |    1    | cancelled_by_driver|2013-10-01|
| 3  |     3     |    12     |    6    |     completed      |2013-10-01|
| 4  |     4     |    13     |    6    | cancelled_by_client|2013-10-01|
| 5  |     1     |    10     |    1    |     completed      |2013-10-02|
| 6  |     2     |    11     |    6    |     completed      |2013-10-02|
| 7  |     3     |    12     |    6    |     completed      |2013-10-02|
| 8  |     2     |    12     |    12   |     completed      |2013-10-03|
| 9  |     3     |    10     |    12   |     completed      |2013-10-03| 
| 10 |     4     |    13     |    12   | cancelled_by_driver|2013-10-03|
+----+-----------+-----------+---------+--------------------+----------+
Users 表存所有用户。每个用户有唯一键 Users_Id。Banned 表示这个用户是否被禁止，Role 则是一个表示（‘client’, ‘driver’, ‘partner’）的枚举类型。
+----------+--------+--------+
| Users_Id | Banned |  Role  |
+----------+--------+--------+
|    1     |   No   | client |
|    2     |   Yes  | client |
|    3     |   No   | client |
|    4     |   No   | client |
|    10    |   No   | driver |
|    11    |   No   | driver |
|    12    |   No   | driver |
|    13    |   No   | driver |
+----------+--------+--------+
写一段 SQL 语句查出 **2013年10月1日 **至 **2013年10月3日 **期间非禁止用户的取消率。基于上表，你的 SQL 语句应返回如下结果，
取消率（Cancellation Rate）保留两位小数。
+------------+-------------------+
|     Day    | Cancellation Rate |
+------------+-------------------+
| 2013-10-01 |       0.33        |
| 2013-10-02 |       0.00        |
| 2013-10-03 |       0.50        |
+------------+-------------------+

select t.Request_at as Day,round(sum(case when t.Status like 'cancelled%' and 'cancelled_by_client' then 1 else 0 end)/count(*),2) as Cancellation Rate from Trips t inner join Users u

on u.Users_Id = t.Client_Id and u.Banned = 'No'

where t.Request_at between '2013-10-01' and '2013-10-03'

group by Trips.Request_at
