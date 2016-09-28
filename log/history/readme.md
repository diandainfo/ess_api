
# 用户浏览商品记录

# 0. Mapping
```
{
    "mappings":{
        "goods":{
            "properties":{
                "gid":{"type":"integer"}
                ,"sid":{"type":"integer"}
                ,"date":{"type":"date","format": "yyyy-MM-dd"}
                ,"time":{"type":"date","format":"yyyy-MM-dd HH:mm:ss:SSS||yyyy-MM-dd HH:mm:ss:SSSZ||yyyy-MM-dd HH:mm:ss:SSSZZ||yyyy-MM-dd HH:mm:ss SSS"}
                ,"timestamp":{"type":"date"}
            }
        }
    }
}
```
***

# 1.1 /goods/set

**简要描述：** 

- 写入用户浏览商品记录/用户足迹

**请求URL：** 
- ` /api/log/history/goods/set `
  
**请求方式：**
- POST ,建议无响应请求

**参数：** 

|参数名|必选|类型|说明|
|:----|:---|:-----|-----|
|sid |是  |int/string |用户编号|
|gid |是  |int/string |商品编号|
|timestamp|否|long/string |13位时间戳，无则使用当前时间|

 **返回示例**
 - 多条写入使用bulk的返回:
 ``` 
    {
        "status": 1,
        "message": "",
        "data": {
            "took": 2,
            "errors": false,
            "items": [
                {
                    "create": {
                        "_index": "history_log_v1",
                        "_type": "goods",
                        "_id": "AVYBF8q8ivQZAyEcTy6N",
                        "_version": 1,
                        "status": 201
                    }
                }
            ]
        }
    }
  ```
  - 单条写入使用create的返回:
  ```
    {
        "status": 1,
        "message": "",
        "data": {
            "_index": "history_log_v1",
            "_type": "goods",
            "_id": "AVduYzdTVrn2t9i4VLca",
            "_version": 1,
            "_shards": {
                "total": 1,
                "successful": 1,
                "failed": 0
            },
            "created": true
        }
    }
  ```

 **返回参数说明** 

- 参见[ElasticSearch标准返回说明](../../README.md#elasticsearch-response)

***

# 1.2 /goods/get

**简要描述：** 

- 读取用户浏览商品记录/用户足迹
    - 根据time反序排列
    - 根据goodId去重
    - 不筛选、过滤已下架商品

**请求URL：** 
- ` /api/log/history/goods/get `
  
**请求方式：**
- POST 

**参数：** 

|参数名|必选|类型|说明|
|:----|:---|:-----|-----|
|sid   |是|int/string |用户编号|
|start |是|string     |开始时间，遵循‘2016-01-01 00:00:00 000’格式，最少传递‘2016-01-01’|
|end   |是|string     |结束时间，同上|
|offset|否|int        |分页：起始量，默认为0|
|limit |否|int        |分页：偏移量，默认为100|

 **返回示例**

``` 
 {
    "status": 1,
    "message": "",
    "count": 6,
    "data": [
        {
            "gid": "33312",
            "sid": "1",
            "time": "2016-07-29 17:12:16"
        },
        {
            "gid": "4",
            "sid": "1",
            "time": "2016-07-20 10:49:31"
        },
        {
            "gid": "3",
            "sid": "1",
            "time": "2016-07-19 14:14:38"
        },
        {
            "gid": "12",
            "sid": "1",
            "time": "2016-07-19 14:14:37"
        },
        {
            "gid": "1",
            "sid": "1",
            "time": "2016-07-19 14:14:36"
        },
        {
            "gid": "11232",
            "sid": "1",
            "time": "2016-07-19 14:14:34"
        }
    ]
}
```

 **返回参数说明** 

|参数名||类型|说明|
|:----|:---|:---|:-----|
|count||int||总计商品浏览记录条数|
|data|sid   |int |用户编号，对应/get中的‘sid’|
||gid   |int     |商品编号，对应/get中的‘gid’|
||time  |string     |记录时间，遵循‘2016-01-01 00:00:00’格式|
