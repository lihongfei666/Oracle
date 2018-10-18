# Oracle
# 实验一：分析SQL执行计划，执行SQL语句的优化指导

## 实验内容：
- 对Oracle12c中的HR人力资源管理系统中的表进行查询与分析。
- 首先运行和分析教材中的样例：本训练任务目的是查询两个部门('IT'和'Sales')的部门总人数和平均工资，以下两个查询的结果是一样的。但效率不相同。
- 设计自己的查询语句，并作相应的分析，查询语句不能太简单。

## 教材中的查询语句

- 查询1：

```SQL
SELECT d.department_name，count(e.job_id)as "部门总人数"，
avg(e.salary)as "平均工资"
from hr.departments d，hr.employees e
where d.department_id = e.department_id
and d.department_name in ('IT'，'Sales')
GROUP BY department_name;
```
运行结果：
![运行结果](https://github.com/lihonfei666/oracle/blob/master/test1/1.png)

- 查询2：
```SQL
SELECT d.department_name，count(e.job_id)as "部门总人数"，
avg(e.salary)as "平均工资"
FROM hr.departments d，hr.employees e
WHERE d.department_id = e.department_id
GROUP BY department_name
HAVING d.department_name in ('IT'，'Sales');
```
运行结果：
![运行结果](https://github.com/lihongfei666/oracle/blob/master/test1/2.png)

>比较两条查询语句，我认为第二条查询语句更优。两条语句都是查询所有部门以及以下员工的总数与平均工资。但是“Where”是一个约束声明，在查询数据库的结果返回之前对数据库中的查询条件进行约束，在结果返回之前起作用；“Having”是一个过滤声明，所谓过滤是在查询数据库的结果返回之后进行过滤，在结果返回之后起作用。

- 自定义查询：
```SQL
SET AUTOTRACE ON
SELECT (R.REGION_NAME)AS "地区名称",COUNT(C.REGION_ID)AS "各地区国家数"
FROM HR.REGIONS R, HR.COUNTRIES C
WHERE R.REGION_ID=C.REGION_ID
group by R.REGION_NAME, R.REGION_ID
HAVING R.REGION_ID IN ('1','2','3','4');
```
运行结果：
![运行结果](https://github.com/lihongfei666/oracle/blob/master/test1/3.png)


>该条自定义查询语句是查询4个地区名称以及各地区的国家数目。

