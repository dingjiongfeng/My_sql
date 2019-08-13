#【任务三】
## **#学习内容#**
数据导入导出
  * 将之前创建的任意一张MySQL表导出,需要是CSV格式
  * 再将CSV表导入数据库
## **#作业#**
### 项目十: 各部门工资最高的员工（难度：中等）
创建Employee 表，包含所有员工信息，每个员工有其对应的 Id, salary 和 department Id。
+----+-------+--------+--------------+
| Id | Name  | Salary | DepartmentId |
+----+-------+--------+--------------+
| 1  | Joe   | 70000  | 1            |
| 2  | Henry | 80000  | 2            |
| 3  | Sam   | 60000  | 2            |
| 4  | Max   | 90000  | 1            |
+----+-------+--------+--------------+
创建Department 表，包含公司所有部门的信息。
+----+----------+
| Id | Name     |
+----+----------+
| 1  | IT       |
| 2  | Sales    |
+----+----------+
编写一个 SQL 查询，找出每个部门工资最高的员工。例如，根据上述给定的表格，Max 在 IT 部门有最高工资，Henry 在 Sales 部门有最高工资。
+------------+----------+--------+
| Department | Employee | Salary |
+------------+----------+--------+
| IT         | Max      | 90000  |
| Sales      | Henry    | 80000  |
+------------+----------+--------+

create table Employee{
    Id int not null primary key,
    Name varchar(50) not null,
    Salary int not null,
    DepartmentId int not null
};

insert into Employee value(1,'Joe',70000,1)

insert into Employee value(2,'Henry',80000,2)

insert into Employee value(3,'Sam',60000,2)

insert into Employee value(4,'Max',90000,1)

create table Department{
    Id int not null primary key,
    Name varchar(50) not null,
};

insert into Department value(1,'IT')

insert into Department value(2,'Sales')

select d.Name AS Department,e.Name AS Employee,e.Salary from Department d,Employee e
where d.Id = e.DepartmentId AND e.Salary >= (SELECT Max(Salary) FROM Employee e2 where e.DepartmentId = e2.DepartmentId)

### 项目十一: 换座位（难度：中等）
小美是一所中学的信息科技老师，她有一张 seat 座位表，平时用来储存学生名字和与他们相对应的座位 id。
其中纵列的 **id **是连续递增的
小美想改变相邻俩学生的座位。
你能不能帮她写一个 SQL query 来输出小美想要的结果呢？
 请创建如下所示seat表：
**示例：**
+---------+---------+
|    id   | student |
+---------+---------+
|    1    | Abbot   |
|    2    | Doris   |
|    3    | Emerson |
|    4    | Green   |
|    5    | Jeames  |
+---------+---------+
假如数据输入的是上表，则输出结果如下：
+---------+---------+
|    id   | student |
+---------+---------+
|    1    | Doris   |
|    2    | Abbot   |
|    3    | Green   |
|    4    | Emerson |
|    5    | Jeames  |
+---------+---------+
**注意：**
如果学生人数是奇数，则不需要改变最后一个同学的座位。


CREATE TABLE IF NOT EXISTS seat(
    id INT PRIMARY KEY,
    student VARCHAR(10) NOT NULL
)Engine=Innodb;

INSERT INTO seat VALUES(1,'Abbot');
INSERT INTO seat VALUES(2,'Doris');
INSERT INTO seat VALUES(3,'Emerson');
INSERT INTO seat VALUES(4,'Green');
INSERT INTO seat VALUES(5,'Jeames');

select (
case
  when mod(id,2) = 1 and id !=max(id) from seat then id - 1
  when mod(id,2) = 0 then id + 1
  else id
) as id,student

from seat order by Id


### 项目十二:  分数排名（难度：中等）
编写一个 SQL 查询来实现分数排名。如果两个分数相同，则两个分数排名（Rank）相同。请注意，平分后的下一个名次应该是下一个连续的整数值。换句话说，名次之间不应该有“间隔”。
创建以下score表：
+----+-------+
| Id | Score |
+----+-------+
| 1  | 3.50  |
| 2  | 3.65  |
| 3  | 4.00  |
| 4  | 3.85  |
| 5  | 4.00  |
| 6  | 3.65  |
+----+-------+
例如，根据上述给定的 Scores 表，你的查询应该返回（按分数从高到低排列）：
+-------+------+
| Score | Rank |
+-------+------+
| 4.00  | 1    |
| 4.00  | 1    |
| 3.85  | 2    |
| 3.65  | 3    |
| 3.65  | 3    |
| 3.50  | 4    |
+-------+------+

select Score,(select count(distinct Score) from Scores as s2 where s2.Score >= s1.Score) Rank from Score

order by score DESC

### 项目十三：连续出现的数字（难度：中等）
编写一个 SQL 查询，查找所有至少连续出现三次的数字。

+----+-----+
| Id | Num |
+----+-----+
| 1  |  1  |
| 2  |  1  |
| 3  |  1  |
| 4  |  2  |
| 5  |  1  |
| 6  |  2  |
| 7  |  2  |
+----+-----+
例如，给定上面的 Logs 表， 1 是唯一连续出现至少三次的数字。

+-----------------+
| ConsecutiveNums |
+-----------------+
| 1               |
+-----------------+

select distinct l1.Num AS ConsecutiveNums from Logs l1,Logs l2,Logs l3

where l2.Id = l1.Id + 1
      and l3.Id = l2.Id + 1
      and l1.Num = l2.Num
      and l2.Num = l3.Num

### 项目十四：树节点 （难度：中等）
对于 **tree** 表，*id* 是树节点的标识，*p_id* 是其父节点的 *id*。

+----+------+
| id | p_id |
+----+------+
| 1  | null |
| 2  | 1    |
| 3  | 1    |
| 4  | 2    |
| 5  | 2    |
+----+------+
每个节点都是以下三种类型中的一种：
* Leaf: 如果节点是根节点。
* Root: 如果节点是叶子节点。
* Inner: 如果节点既不是根节点也不是叶子节点。

写一条查询语句打印节点id及对应的节点类型。按照节点id排序。上面例子的对应结果为：
+----+------+
| id | Type |
+----+------+
| 1  | Root |
| 2  | Inner|
| 3  | Leaf |
| 4  | Leaf |
| 5  | Leaf |
+----+------+
**说明**
* 节点’1’是根节点，因为它的父节点为NULL，有’2’和’3’两个子节点。
* 节点’2’是内部节点，因为它的父节点是’1’，有子节点’4’和’5’。
* 节点’3’，‘4’，'5’是叶子节点，因为它们有父节点但没有子节点。

下面是树的图形：

|         1      /   \    2       3  /   \4       5 | 
|----|
**注意**
如果一个树只有一个节点，只需要输出根节点属性。

select id,() as Type from tree 



### 项目十五：至少有五名直接下属的经理 （难度：中等）
**Employee** 表包含所有员工及其上级的信息。每位员工都有一个Id，并且还有一个对应主管的Id（ManagerId）。
| 12345678910 | +------+----------+-----------+----------+\|Id    \|Name 	  \|Department \|ManagerId \|+------+----------+-----------+----------+\|101   \|John 	  \|A 	      \|null      \|\|102   \|Dan 	  \|A 	      \|101       \|\|103   \|James 	  \|A 	      \|101       \|\|104   \|Amy 	  \|A 	      \|101       \|\|105   \|Anne 	  \|A 	      \|101       \|\|106   \|Ron 	  \|B 	      \|101       \|+------+----------+-----------+----------+ | 
|----|----|
针对 **Employee** 表，写一条SQL语句找出有5个下属的主管。对于上面的表，结果应输出：
| 12345 | +-------+\| Name  \|+-------+\| John  \|+-------+ | 
|----|----|
**注意:**
没有人向自己汇报。

select Name
from(
select ManagerId,count(Id) as cou 
from Employee
group by ManagerId) m,Employee e
where m.ManagerId=e.Id and cou>=5
# 

