
# LBS - 业务员信息

# 1.1 /update

**简要描述：** 
- 更新单个业务员的信息

**请求URL：** 
- ` /api/lbs/salesman/update `
  
**请求方式：**
- POST 

**参数：** 

|参数名|必选|类型|说明|
|:----|:---|:-----|-----|
|mid   |是|int |业务员编号|
|-|是|-|以下多个字段中必须输入一个以进行修改操作,且输入不能为空|
|cid|否|int|cityId，城市编号|
|mna|否|string|业务员姓名|
|state|否|int|状态，1为“活着”|
|phone|否|string|手机号|

**返回示例**

- 请求：

	```
	 /api/lbs/salesman/update -d '
	{
		mid:188
		,cid:320400
	}'
	```
- 时，返回

	``` 
	 {
	    "status": 1,
	    "message": "",
    	"data": true
	}
	```

- 当某个值修改为空时，返回
	```
	{
	    "status": -1,
	    "message": "7002:必须输入属性字段"
	}
	```

**返回参数说明** 

|参数名|类型|说明|
|:----|:---|:-----|
|status|int|是否成功|

# 1.2 /list

**简要描述：** 
- 获取“活着”的业务员列表

**请求URL：** 
- ` /api/lbs/salesman/list `
  
**请求方式：**
- POST 

**参数：** 

|参数名|必选|类型|说明|
|:----|:---|:-----|-----|
|mid   |否|int |指定查询的业务员编号,支持输入多个mid，以英文 ’,’ 逗号分开，如： 1,2,3|
|cid|否|int|cityId，城市编号|
|offset|否|int|起始量，和limit一起进行分页|
|limit|否|int|偏移量|
|sort|否|string|排序列，若该值不存在于列中，则不排序，此排序为分页内排序，整体排序为[1,10,102,11,2]方式|
|sb|否|stirng|排序方式，`desc`为反序，其他为正序|

**返回示例**

- 成功返回:

	``` 
	{
	    "status": 1,
	    "message": "",
	    "data": [
	        {
	            "mid": "1",
	            "createdAt": "2016-10-19 22:29:40",
	            "updatedAt": "2016-10-19 22:29:40",
	            "activeAt": 1476887380,
	            "lat": 0,
	            "lon": 0,
	            "phone": "13111111111",
	            "name": "xxxx",
	            "state": 1
	        }
	    ],
	    "count": 123
	}
	```

**返回参数说明** 

|参数名||类型|说明|
|:----|:---|:---|:-----|
|count||int|总计符合条件的店铺搜索结果条数|
|data|   |     ||
||mid  |int     |业务员编号|
||createdAt|string|本记录创建时间|
||updatedAt|string|本条记录更新时间|
||activeAt|long|该业务员最后一个轨迹点的时间戳，即最后活跃时间|
||lat|double|最后一个轨迹点的纬度|
||lon|double|最后一个轨迹点经度|
||phone|string|手机号|
||name|string|业务员姓名|
||state|int|当前状态|

