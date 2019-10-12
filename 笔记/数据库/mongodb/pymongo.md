# pymongo

**1.pymongo使用：**

```python
import pymongo
client = pymongo.MongoClient(host='',port='')
# 指定数据库
db = client.test/client['test']
# 指定集合
collection = db.test1/db.['test1']
# 插入数据
result = collection.insert(data)
# 查询数据
data = collection.find_one({'key':'value'})
# 总数计算
# 更新
# 删除
```

