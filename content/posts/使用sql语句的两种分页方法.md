---  
title: 使用sql语句的两种分页方法  
date: 2015-10-19 15:12:58+08:00  
tags:   
canonicalURL: https://blog.csdn.net/sinat_32124195/article/details/49250085  
share: true  
slug: sql-page  
---  
  
## 使用存储过程  
### 1. 使用 top 方法  
```sql  
<pre name="code" class="sql">--一条一条的访问数据库  
Create procedure data_page  
@num int,--每页的信息数  
@i int--接受是第几页  
as  
declare @n int  
--存储总信息数  
set @n=(select COUNT(*) from people)  
if(@i<=@n/@num)  
begin  
select top (@num) * from people   
where P_id in(select top (@i*@num) p_id from people order by p_id asc)   
order by p_id desc  
end  
--如果最后一张不是完整的一页  
--进入一面的判断  
if(@i*@num-@n>0)  
	select top (@n-(@i-1)*@num) *   
	from people order by p_id desc  
go  
exec data_page 5,1  
--删除存储过程  
drop procedure data_page  
```  
  
### 2.使用 sql 里面的内置函数 row_number(可以生成行号)  
```SQL  
<pre name="code" class="sql">--内置函数ROW_NUMBER()的使用  
--给每行加编号  
select *,row_number() over(order by p_id) as rows from people  
--按组进行编号,以5行为一组  
select *,((ROW_NUMBER() over(order by p_id)-1)/5 as rows from people  
go  
--利用编号进行分页  
   
create procedure proSplitPage  
@peerPageRows int,--每页要显示的行数  
@indexPage int--接受要显示第几页  
as  
   
select p_id,p_name,p_pwd,p_age,register_time from   
(select *,(ROW_NUMBER() over(order by p_id))-1)/@peerPageRows  as rows  
from people) as tb where rows=@indexPage-1  
```