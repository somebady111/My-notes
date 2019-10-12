# sql编程

**1.语句示例：**

```sql
select time as '时间(time)', case is_success when '0' then  'fault' when '1' then 'completed' end as '结果(result)' ,count(time) as '个数(count)' from sheet1 where is_success in ("0","1")  group by time;
根据time分组根据条件字段is_success在0或1从sheet1表中查询组字段time别名为'时间(time)'，当且仅当字段is_success为0是别名为fault，当为1时别名为completed结果别名为'结果(result)'总和别名为'个数(count)'
```

