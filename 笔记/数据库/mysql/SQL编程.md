# SQL编程

```sql
# 基本查找
select list from table; 查找某列
# 比较值
select is_success from sheet1 where is_success<>0; 查找不等于0的，这里的不等于号按照国际表准来使用，也可以是!=
select is_success from sheet1 where is-success>any(1,2,3); 查找大于列表中的数(比较任意一个值)),也可用some
select is_success from sheet1 where is-success>any(1,2,3); 查找大于列表中的数(比较所有的值)
# 查询别名
select list 列表 from table; 查找使用列表别名,或者加as
# 查询进制过滤重复行
select distinct is_success from sheet1;
# 使用SQL操作符
# like操作符
select * from sheet1 where time like '%32%'; # 匹配查询指定位置的一个字符，%匹配任意多个字符，也可使用not like不匹配
# in操作符
select * from sheet1 where is_success in (0); # 列表中不能有null值，否则返回false,也可是not in
# between操作符
select * from sheet1 where is_success between 1 and 2; # not between
# 逻辑操作符
and 同时满足
or 满足其中之一
not 相反
# order by 子句查询，对查询结果进行排序
select * from sheet1 order by time; # 默认为升序排序
select * from sheet1 order by time desc; # 降序排序
# 多表查询

```

