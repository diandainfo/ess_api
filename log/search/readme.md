
# 搜索关键词日志

# 0. mapping
```
{
    "mappings": {
        "search_keyword": {
            "properties": {
                "keyword": {"type": "string", "index": "not_analyzed"}
                , "cityId": {"type": "string", "index": "not_analyzed"}
                , "sid": {"type": "integer"}
                , "count": {"type": "integer"}
                , "timestamp": {"type": "date"}
            }
        }
    }
}
```
***

# 1.1 /get

**简要描述：** 

- 获取用户搜索关键词记录

**请求URL：** 
- ` /api/log/search/get `
  
**请求方式：**
- POST

**参数：** 

|参数名	|必选  	|类型		|说明					|
|:----	|:-----	|:-----		|-----					|
|sid 	|是		|int/string	|用户编号				|
|offset	|否		|int		|起始量，默认为0			|
|limit	|否		|int 		|偏移量，默认为10			|
|type	|否		|string		|查询类型，默认为 `simple`，只返回关键词信息; `full`时显示关键词的搜索结果数和时间戳|

 **返回示例**
* `type` 为 `simple` 时	
  ``` 
	{
	    "status": 1,
	    "message": "",
	    "data": {
	        "count": 19,
	        "data": [
	            "奥妙",
	            "奥利奥",
	            "寿司",
	            "可",
	            "可比克",
	            "康师傅",
	            "可乐",
	            "散娃",
	            "散",
	            "顺心"
	        ]
	    }
	}
  ```

* `type` 为 `full` 时
  ```
	{
	    "status": 1,
	    "message": "",
	    "data": {
	        "count": 19,
	        "data": [
	            {
	                "keyword": "奥妙",
	                "timestamp": 1471327197006,
	                "count": 31
	            },
	            {
	                "keyword": "奥利奥",
	                "timestamp": 1471327191704,
	                "count": 55
	            }
	        ]
	    }
	}
  ``` 
 **返回参数说明** 

|参数名||类型|说明|
|:----|:---|:---|:-----|
|count||int|搜索记录条数|
|data|keyword   |string     |搜索关键词|
||timestamp  |long     |搜索时间戳|
||count  |int     |搜索结果数量|

***

# 1.2 /goods

**简要描述：** 

- 获取(分城市)关键词聚合信息

**请求URL：** 
- ` /api/log/search/goods `
  
**请求方式：**
- POST

**参数：** 

|参数名	|必选  	|类型		|说明					|
|:----	|:-----	|:-----		|-----					|
|cityId |否		|string		|城市编号，无则全局进行聚合	|
|offset	|否		|int		|起始量，默认为0			|
|limit	|否		|int 		|偏移量，默认为50			|
|start	|否		|string/long|起始时间，取值范围为'2016-06-01 11:11:11'或13位时间戳；单使用日期，如'2016-06-11'时，根据'2016-06-11 00:00:00'进行查询|
|end	|否		|string/long|结束时间，取值范围同上|
 **返回示例**
```
{
    "status": 1,
    "message": "",
    "data": [
        {
            "key": "娃哈哈",
            "count": 4
        },
        {
            "key": "口水娃",
            "count": 1
        },
        {
            "key": "可",
            "count": 1
        }
    ],
    "count": 19
}
```
 **返回参数说明** 

|参数名||类型|说明|
|:----|:---|:---|:-----|
|count||int|搜索记录条数|
|data|key   |string     |搜索关键词|
||count  |int     |搜索次数|