# 0. /log_info/_mapping
```
{
    "mappings":{
        "back":{
            "properties":{
                "system":{"type":"string","index":"not_analyzed"}
                ,"message":{"type":"string"}
                ,"ip":{"type":"ip"}
                ,"date":{"type":"date","format": "yyyy-MM-dd"}
                ,"time":{"type":"date","format":"yyyy-MM-dd HH:mm:ss:SSS||yyyy-MM-dd HH:mm:ss:SSSZ||yyyy-MM-dd HH:mm:ss:SSSZZ||yyyy-MM-dd HH:mm:ss SSS"}
                ,"timestamp":{"type":"date"}
            }
            ,"_ttl":{
                "enabled":true,
                "default": "7d"
            }
        }
    }
}
```
***

# 1.1 /back/set

**简要描述：** 

- 写入后台系统错误日志
- 错误日志仅保留`7`天

**请求URL：** 
- ` /api/log/info/back/set `
  
**请求方式：**
- POST ，建议无响应请求

**参数：** 

|参数名|必选|类型|说明|
|:----|:---|:-----|-----|
|system |是|string|系统标识，用于分辨系统|
|message|是|string|错误日志内容|
|ip|否|ipv4/string |ip地址，无则使用发信端ip|
|timestamp|否|long/string |13位时间戳，无则使用当前时间|

 **返回示例**

``` 
  {
    "status": 1,
    "message": "",
    "data": {
        "_index": "log_info_v1",
        "_type": "back",
        "_id": "AVYHR1TWivQZAyEcTy7G",
        "_version": 1,
        "created": true
    }
}
```

 **返回参数说明** 

- 参见[ElasticSearch标准返回说明](../../README.md#elasticsearch-response)
- data中created为true说明创建成功，否则message会返回具体错误信息
- 请求调用方法说明参见：[Elasticsearch.js中create方法说明](https://www.elastic.co/guide/en/elasticsearch/client/javascript-api/current/api-reference.html#api-create)
- 返回参数说明参见：[Elasticsearch中Index Api说明](https://www.elastic.co/guide/en/elasticsearch/reference/2.3/docs-index_.html)

***

# ~~1.2 /back/get~~

**重定向到** /log/info 页面链接 
