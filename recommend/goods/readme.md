# 推荐 - 商品 - 列表

# 1.1 /list

**简要描述：** 

- 获取推荐商品列表
    - 根据公式进行计算
    - 只返回在售商品
    - 数据更新周期为1d

**请求URL：** 
- ` /api/recommend/goods/list `
  
**请求方式：**
- POST 

**参数：** 

|参数名|必选|类型|说明|
|:----|:---|:-----|-----|
|ti   |是|int |typeIndex,排序分数的类型序号,取值为0-9|
|cid  |是  |string |店铺所在城市编号，即xxxx00六位数字，如320400|
|fci  |否  |int/string |商品一级分类编号|
|nfi  |否  |Array:int/string |必须为数组，补充参数:推荐中去除的商品一级分类编号--使用[must_not](https://www.elastic.co/guide/en/elasticsearch/reference/1.4/query-dsl-bool-query.html)+[terms](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-terms-query.html)实现|
|sci  |否  |int/string |商品二级分类编号|
|dir  |否  |string |排序方式，取值为'desc'、'asc'|
|offset|否|int |起始量，默认为0，和limit搭配使用，用于分页,取值0-10000| 
|limit|否|int |偏移量/记录条数，默认为20，即需要返回的记录数量，取值0-10000| 
|ft  |否  |string |filedType,返回参数,可选'all'，默认不填则只返回gid列表，'all'时返回score信息|

 **返回示例**

  - 请求:

	```
	 /api/recommend/goods/list -d '
	{
		ti:0
		,cid:320400
		,ft:'all'
		,limit:1
	}'
	```
   - 时,返回

	``` 
	 {
	    "status": 1,
	    "message": "",
	    "count":125,
	    "data": [
	        {
	            "gid": 148095,
	            "score": 72.38054635740531,
	            "cid": 320400,
	            "fci": 2000000,
	            "sci": 2010000,
	            "name": "【2件售卖】康师傅包装饮用水550ml*12瓶/箱"
	        }
	    ]
	}
	```
	
  - 请求:
  	
	```
	/api/recommend/goods/list -d '
	{
		ti:0
		,cid:320400
	}'
	```
	
    - 时,返回:
	
	```
	{
	    "status": 1,
	    "message": "",
	    "count":256,
	    "data": [
	        148095,
	        148219,
	        1190,
	        130191,
	        121168,
	        229,
	        120666,
	        142681,
	        383,
	        149091,
	        122540,
	        119986,
	        147771,
	        132921,
	        146659,
	        1898,
	        122391,
	        119995,
	        123138,
	        148711
	    ]
	}
	```
	
 **返回参数说明** 

|参数名||类型|说明|
|:----|:---|:---|:-----|
|count||int|总计符合条件的推荐商品数量|
|data|||ft不为'all'时,返回gid数组|
||gid   |int     |商品编号|
||name  |string     |商品名称|
||cid  |inte     |城市编号|
||fci  |int     |一级分类编号|
||sci  |int     |二级分类编号|


