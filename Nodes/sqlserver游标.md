```
declare @user_id int
declare @dept_id int
declare mycursor cursor
for 
    select u.user_id,d.dept_id from sys_user u,sys_dept d where u.dept_id = d.dept_id
open mycursor
fetch next from mycursor into @user_id,@dept_id
while @@FETCH_STATUS=0
    begin
		insert into A_User_Role_Test(user_id,role_id) values(@user_id,4)
		fetch next from mycursor into @user_id,@dept_id
	end 
close mycursor
deallocate mycursor
```

[SQL SERVER 游标使用](https://blog.csdn.net/sinat_28984567/article/details/79811887?utm_medium=distribute.pc_relevant.none-task-blog-2~default~BlogCommendFromMachineLearnPai2~default-2.control&dist_request_id=1331303.5828.16182723155861753&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2~default~BlogCommendFromMachineLearnPai2~default-2.control)
---

我们在处理数据的时候，经常会出现需要循环处理数据的需求，如果我们能用CTE或者其他语句处理的话，没有问题，但有时候往往处理起来比较困难，这时候我们可以选择使用游标处理，选择使用哪种形式，要考虑效率问题，一般游标效率不高，但也有适合使用的场景。

游标分为静态游标和动态游标，静态游标的数据是固定的，不会因为数据表的改变而改变；动态游标的数据是随着数据表变化而变化的，游标默认是动态游标，通过关键字STATIC设置，OK，上测试数据：

```
--测试数据
if not object_id(N'Tempdb..#T') is null
	drop table #T
Go
Create table #T([id] int,[name] nvarchar(22))
Insert #T
select 1,N'张三' union all
select 2,N'李四' union all
select 3,N'王五' union all
select 4,N'赵六'
Go
--测试数据结束
```
我们先看静态游标的使用方法：
```
DECLARE @id INT , @name NVARCHAR(50)    --声明变量，需要读取的数据
DECLARE cur CURSOR STATIC				--声明静态游标
FOR
    SELECT  * FROM    #T
OPEN cur								--打开游标
FETCH NEXT FROM cur INTO @id, @name     --取数据
WHILE ( @@fetch_status = 0 )            --判断是否还有数据
    BEGIN
        SELECT '数据: ' + RTRIM(@id) + @name
		UPDATE #T SET name='测试' WHERE id=4  --测试静态动态用
        FETCH NEXT FROM cur INTO @id, @name   --这里一定要写取下一条数据
    END
CLOSE cur								--关闭游标
DEALLOCATE cur
```
结果如下，我们可以看到ID是4的数据没有改变，依然是赵六，而不是UPDATE之后的测试：
![img](../20180404091200609)

我们再来看一下，动态游标，去掉STATIC关键字即可：

```
DECLARE @id INT , @name NVARCHAR(50)    --声明变量，需要读取的数据
DECLARE cur CURSOR 				--去掉STATIC关键字即可
FOR
    SELECT  * FROM    #T
OPEN cur								--打开游标
FETCH NEXT FROM cur INTO @id, @name     --取数据
WHILE ( @@fetch_status = 0 )            --判断是否还有数据
    BEGIN
        SELECT '数据: ' + RTRIM(@id) + @name
		UPDATE #T SET name='测试' WHERE id=4  --测试静态动态用
        FETCH NEXT FROM cur INTO @id, @name   --这里一定要写取下一条数据
    END
CLOSE cur								--关闭游标
DEALLOCATE cur
```
![img](../20180404091349153)
以上是游标的用法，以及动态、静态游标的介绍使用。

[Sql中CHARINDEX用法](https://www.cnblogs.com/qianxingdewoniu/p/6858580.html)
---
- CHARINDEX作用
写SQL语句我们经常需要判断一个字符串中是否包含另一个字符串，但是SQL SERVER中并没有像C#提供了Contains函数，不过SQL SERVER中提供了一个叫CHAEINDX的函数，顾名思义就是找到字符（char）的位置（index），既然能够知道所在的位置，当然就可以判断是否包含在其中了。
通过CHARINDEX如果能够找到对应的字符串，则返回该字符串位置，否则返回0。

基本语法如下：
CHARINDEX ( expressionToFind , expressionToSearch [ , start_location ] )
expressionToFind ：目标字符串，就是想要找到的字符串，最大长度为8000 。
expressionToSearch ：用于被查找的字符串。
start_location：开始查找的位置，为空时默认从第一位开始查找。

- PATINDEX
和CHARINDEX类似，PATINDEX也可以用来判断一个字符串中是否包含另一个字符串，两种的差异在于，前者是全匹配，后者支持模糊匹配。
	1. 简单示例
select PATINDEX('%ter%','interesting data')
	2. 简单示例2
select PATINDEX('%t_ng%','interesting data')

PATINDEX也允许支持大小写敏感，做法和CHARINDEX一样，此处不再累述。










