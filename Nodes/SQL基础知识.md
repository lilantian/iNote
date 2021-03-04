[SQL基础知识](https://blog.csdn.net/u010565545/article/details/100785261/)

SQL基础知识整理：
```
select 查询结果 如：【学号，平均成鸡：组函数avg（成鸡）】
from 从哪张表中查询数据 如：【涉及到成绩：成绩表score】
where 查询条件 如：【b.课程号=‘003’ and b.成绩>80】
group by 分组 如：【每个学生的平均：按学号分组】（oracle,SQL server中出现在select子句后的非分组函数，必须出现在group by子句后出现），MySQL中可以不用
having 对分组结果指定条件 如：【大于60分】
order by 对查询结果排序 如：【增序；成绩 ASC/降序：成绩 DESC】
limit 使用limit子句返回topN（对应这个问题返回的成绩前两名） 如：【limit 2 ==>从0索引开始读取2个】
limit==>从0索引开始[0,N-1]
```
```
select * from table limit 2,1;
//含义是跳过2条取出1条数据，limit后面是从第2条开始读，读取一套信息，即读取第3条数据
select * from table limit 2 offset 1;
//含义是从第1条（不包括）数据开始取出2条数据，limit后面跟的是2条数据，offset后面是从第1条开始读取，即读取第2,3条
```
组函数：
- 去重distinct()
- 统计总数sum()
- 计算个数count()
- 平均数avg()
- 最大值max()
- 最小值min()

多表连接：
- 内连接（省略默认inner）join...on...
- 左连接 left join tableName as b on a.key == b.key
- 右连接 right join 
- 连接 union（无重复（过滤去重））和union all(有重复【不过滤去重】)


-- union 并集
-- union all（有重复）
-- intersect 交集
-- minus(except)相减（差集）

---
Oracle  
## 一、对象：表(table) 视图(view) 序列(sequence) 索引(index) 同义词(synonym)

### 1.视图：存储起来的select语句
create view emp_vw
as
select employee_id, last_name, salary
from employees
where department_id = 90;
select * from emp_vw;

### --可以对简单视图进行DML操作
update emp_vw
set last_name = 'HelloKitty'
where employee_id = 100;

select * from employees
where employee_id = 100;

### 1)复杂视图
create view emp_vw2
as
select department_id, avg(salary) avg_sal
from employees
group by department_id;

select * from emp_vw2;

### --复杂视图不能进行DML操作
update emp_vw2
set avg_sal = 10000
where department_id = 100;

### 2.序列：用于生成一组有规律的数值。（通常用于为主键设置值）
create sequence emp_seq1
start with 1
increment by 1
maxvalue 10000
minvalue 1
cycle
nocache;

select emp_seq1.currval from dual;

select emp_seq1.nextval form dual;

### --问题：裂缝。 原因：1.当多个表公用同一个序列时。 2.rollback 3.发生异常
create talbe emp1(
	id number(10),
	name vachar2(30)
);

insert into emp1
values(emp_seq1.nextval, '张三')；

select * from emp1;

### 3.索引：提高查询效率
-- 自动创建：Oracle会为具有唯一约束（唯一约束，主键约束）的列，自动创建索引
create table emp2(
	id number(10) primary key;
	name varchar2(30)
);

### --手动创建
create index emp_idx
on emp2(name);

create index emp_idx2
on emp2(id, name);

### 4.同义词
create synonym d1 for departments;
select * from d1;

### 5.表
DDL：数据定义语言 create talbe .../drop talbe .../rename ... to .../truncate talbe .../alter table ...
DML：数据操纵语言 insert into ... values .../update ... set ... where .../ delete from ... where ...

【重要】
select ...组函数（MIN()/MAX()/SUM()/AVG()/COUNT()）
from ... join ... on ... 左外连接：left join ... on ... 右外连接：right join ... on ...
where ...
group by ... (oracle,SQL server中出现在select子句后的非分组函数，必须出现在group by子句后)
having ... 用于过滤组函数
order by ... asc升序， desc降序
limit(0,4) 限制N条数据 如：topN数据

```
union 并集
union all（有重复）
intersect 交集
minus 相减
```

DCL：数据控制语言  commit：提交 / rollback：回滚/授权grant...to... /revoke

何时创建索引：
1. 主键自动建立唯一索引
2. 频繁作为查询条件的字段应该创建索引
3. 查询中与其他表关联的字段，外键关系建立索引
4. 频繁更新的字段不适合创建索引
5. Where条件里用不到的字段不创建索引
6. 单键/组合索引的选择问题，who？（在高并发下倾向创建组合索引）
7. 查询中排序的字段，排序字段若通过索引去访问将大大提高排序速度
8. 查询中统计或者分组字段

问题：查询工资大于149号员工工资的员工的信息
select * 
from employees
where salary > (
	select salary
	from employees
	where employee_id = 149
)

问题：查询与141号或174号员工的manager_id和department_id相同的其他员工的employee_id, manager_id, department_id
select employee_id, manager_id, department_id
from employees
where manager_id in (
	select manager_id
	from employees
	where employee_id in(141, 174)
) and department_id in (
	select department_id
	from employees
	where employee_id in (141, 174)
) and employee_id not in (141, 174)

问题：返回比本部门平均工资高的员工的last_name, department_id, salary及平均工资
select last_name, department_id, salary, 
	(select avg(salary) from employees where department_id = e1.department_id)
from employees e1
where salary > (
	select avg(salary)
	from employees e2
	where e1.department_id = e2.department_id
)

select last_name, e1.department_id, salary, avg_sal
from employees e1, (
	select department_id, avg(salary) avg_sal
	from employees
	groupby department_id
) e2
where e1.department_id = e2.department_id
and e1.salary > e2.avg_sal;

-- case ... when ... then ... when ... then ... else ... and
-- 查询：若部门为10查看工资的1.1倍，部门号为20工资的1.2倍，其余1.3倍
select employee_id, last_name, salary, 
	case department_id 
	when 10	then salary * 1.1 
	when 20 then salayr * 1.2
	else salary * 1.3
	end "new_salary"
from employees;
	
问题：查询员工的employee_id, last_name，要求按照员工的department_name排序
select emplyee_id,last_name
from employees e1
order by (
	select department_name
	from departments d1
	where e1.department_id = d1.department_id
)

--SQL优化：能使用EXISTS就不要使用IN
问题：查询公司管理者的employee_id, last_name, job_id, department_id信息
select employee_id, last_name, job_id, department_id
from employees
where employee_id in (
	select manager_id
	from emplyees
)

select employee_id, last_name, job_id, department_id
from employees e1
where exists (
	select 'X'
	from employees e2
	where e1.employee_id = e2.manager_id
)

问题：查询departments表中，不存在与employees表中的部门的department_id和department_name
select department_id, department_name
from departments d1
where not exists(
	select 'x'
    from employees e1
    where e1.department_id = d1.department_id
)

更改108员工的信息：使其工资变为所在部门中的最高工资，job变为公司中的平均工资最低的job
update employees e1
set salary = (
	select max(salary)
	from employees e2
	wehre e1.department_id = e2.department_id
), job_id = (
	select job_id
	from employees
	group by job_id
	having avg(salary) = (
		select min(avg(salary))
		from employees
		group by job_id
	) 
)
where employee_id = 108;

删除108号员工所在部门中工资最低的那个员工
delete from employees e1
where salary = (
	select min(salary) 
	from emnployees
	where deparetment_id = (
		select department_id
		from employees
		where employee_id = 108
	)
)

select * from employees where employee_id = 108;
select * from employees where department_id = 100
order by salary;

rollback;

查询姓“猴”的学生名单
select * from student where name like '猴%'；
查询姓名中最后一个字是“猴”的学生名单
select * from student where name like '%猴'；
查询姓名中带“猴”的学生名单
select * from student where name like '%猴%'；
查询姓“孟”老师的个数
select count(教师号) from teacher where name like '孟%'

查询课程编号为‘0002’的总成绩
select sum(成绩) from score where 课程号 = '0002';
查询选了课程的学生人数
select count(distinct 学号) as 学生人数 from score;

查询各科成绩最高和最低的分，以如下的形式显示：课程号，最高分，最低分
select 课程号, max(成绩) as 最高分, min(成绩) as 最低分
from score
group by 课程号；

查询每门课程被选修的学生数
select 课程号, count(学号)
from score
group by 课程号;

查询男生、女生人数
select 性别,count(*)
from student
group by 性别;

查询平均成绩大于60分学生的学号和平均成绩
select 学号, avg(成绩)
from score
group by 学号
having avg(成绩)>60;

查询至少选修两门课程的学生学号
select 学号, count(课程号) as 学修课程数目
from score
group by 学号
having count（课程号）>= 2;

查询同名同姓学生名单并统计人数
select 姓名, count(*) as 人数
from student
group by 姓名
having count(*) >= 2;

查询不及格的课程并按课程号从大到小排列
select 课程号
from score
where 成绩 < 60
order by 课程号 desc;

查询每门课程的平均成绩，结果按平均成绩升序排列，平均成绩相同时，按课程号升序排列
select 课程号, avg(成绩) as 平均成绩
from score
group by 课程号
order by 平均成绩 asc, 课程号 desc;

检索课程编号为“0004”且分数小于60的学生学号，结果按分数降序排列
select 学号
from score
where 课程号 = ‘04’ and 成绩 < 60
order by 成绩 desc;

统计每门课程的学生选修人数（超过2人的课程才统计）
要求输出课程号和选修人数，查询结果按人数降序排列，若人数仙童，按课程号升序排列
select 课程号, count(学号) as '选修人数'
from score
group by 课程号
having count(学号) > 2
order by count(学号) desc, 课程号 asc;

查询所有学生的学号、姓名、选课数、总成绩
select a.学号, a.姓名, count(b.课程号) as 选课书, sum(b.成绩) as 总成绩
from student as a left join score as b
on a.学号 = b.学号
group by a.学号；

查询平均成绩大于85的所有学生的学号、姓名和平均成绩
select a.学号, a.姓名, avg(b.成绩) as 平均成绩
from student as a left join score as b
on a.学号 = b.学号
group by a.学号
having avg(b.成绩) > 85;

查询学生的选课情况：学号，姓名，课程号，课程名称
select a.学号, a.姓名, c.课程号, c.课程名称
from student a inner join score b on a.学号 = b.学号
inner join coures c on b.学号 = c.学号;

查询出每门课程的及格人数和不及格人数
select 课程号, 
sum(case when 成绩 >= 60 then 1
	else 0
	end) as 及格人数,
sum(case when 成绩 < 60 then 1
	else 0
	end) as 不及格人数
from score
group by 课程号；

使用分段[100-85],[85-70],[70-60],[<60]来统计各科成绩，分别统计：各分数段人数，课程号和课程名称
select a.课程号,b.课程名称,
sum(case when 成绩 between 85 and 100 
	 then 1 else 0 end) as '[100-85]',
sum(case when 成绩 >=70 and 成绩<85 
	 then 1 else 0 end) as '[85-70]',
sum(case when 成绩>=60 and 成绩<70  
	 then 1 else 0 end) as '[70-60]',
sum(case when 成绩<60 then 1 else 0 end) as '[<60]'
from score as a right join course as b 
on a.课程号=b.课程号
group by a.课程号,b.课程名称;








