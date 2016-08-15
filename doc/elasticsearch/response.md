# Elasticsearch Response
**ElasticSearch请求的标准返回说明**
 
 * **返回示例**

``` 
"data": {
        "took": 2,
        "errors": true, 
        "items": [
            {
                "create": {
                    "_index": "history_log_v1",
                    "_type": "goods",
                    "_id": "AVYBF8q8ivQZAyEcTy6N",
                    "_version": 1,
                    "status": 201
                }
            }, {
                "create": {
                    "_index": "history_log_v1",
                    "_type": "goods",
                    "_id": "AVYBM1iwivQZAyEcTy6O",
                    "status": 400,
                    "error": "MapperParsingException[failed to parse [date]]; nested: MapperParsingException[failed to parse date field [Invalid date], tried both date format [yyyy-MM-dd], and timestamp number with locale []]; nested: IllegalArgumentException[Invalid format: \"Invalid date\"]; "
                }
            }
        ]
    }
```

* **返回参数说明** 参见[《更省时的批量操作》](http://es.xiaoleilu.com/030_Data/55_Bulk.html)
 
|参数名||类型|说明|
|:----|:-----|:-----|-----|
|took||integer|核心用时(ms)，调用es服务进行运算耗时|
|errors||boolean|是否有错误，当提交数据为数组时，其中一个错误则此值为true，否则false即为全部成功|
|items|_index|string|索引名称|
||_type|string|类型名称|
||_id|string|索引编号(非指定则随机生成)|
||status|integer|状态|
||error|string|错误说明|
 
