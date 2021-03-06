
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
|active|否|long|s级时间戳，10位，指定该字段时,返回从该时间点之后仍有位置变动的业务员|
|cid|否|int|cityId，城市编号|
|offset|否|int|页数，不是起始量，是第几页，从1开始|
|limit|否|int|每页记录数量|
|sort|否|string|排序列，若该值不存在于列中，则不排序，此排序为分页内排序，整体排序为[1,10,102,11,2]方式|
|sb|否|stirng|排序方式，默认反序，`desc`为反序，其他为正序|
|location|否|string|有值，则查询业务员最后一个轨迹点的定位地址|

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
||address|int|当location参数存在时，此值为最后一次定位点地址，否则此列不存在|

# 1.3 /delete

**简要描述：** 
- 批量删除业务员entity

**请求URL：** 
- ` /api/lbs/salesman/delete `
  
**请求方式：**
- POST 

**参数：** 
- **单个删除的时候，同时解除店铺和业务员的绑定关系**

|参数名|必选|类型|说明|
|:----|:---|:-----|-----|
|mids   |是|int |指定查询的业务员编号,支持输入多个mid，使用数组，如： [1,2,3]|


**返回示例**

- 批量删除，成功返回:

	``` 
	{
	    "status": 1,
	    "message": "",
	    "data": {
	        "total": 2,
	        "success": 1,
	        "fail": 1,
	        "results": [
	            {
	                "mid": 213213,
	                "message": "删除失败指定track不存在"
	            }
	        ]
	    }
	}
	```
- 单个删除，成功返回

	```
	{
	    "status": 1,
	    "message": "",
	    "data": {
	        "deleted": true,
	        "release": {
	            "total": 15,
	            "success": 15,
	            "results": []
	        }
	    }
	}
	```

**返回参数说明** 

- 批量删除业务员结果:

	|参数名||类型|说明|
	|:----|:---|:---|:-----|
	|data|   |     ||
	||total|int|总计需要删除的mid数量|
	||success|int|成功的数量|
	||failed|int|失败的数量|
	||results|array|失败的原因|
	
- 单个删除业务员，同时删除
	
	|参数名|||类型|说明|
	|:--|:--|:--|:--|:--|
	|data||||结果集|
	||deleted||boolean|是否删除单个业务员成功|
	||release||object|解除店铺业务员关系结果|
	|||total|int|该业务员绑定的店铺总数|
	|||success|int|成功解除绑定关系的店铺数量|
	|||results|array|**ES标准返回,不需要分析**，未解除店铺关系的店铺信息|
	
# 1.4 /add

**简要描述：** 
- 新增业务员entity

**请求URL：** 
- ` /api/lbs/salesman/add `
  
**请求方式：**
- POST 

**参数：** 

|参数名|必选|类型|说明|
|:----|:---|:-----|-----|
|mid   |是|int |业务员编号|
|cid|是|int|城市编号|
|mt|是|int|业务员状态|
|mp|是|string|业务员手机号|
|mna|是|string|业务员姓名|

**返回示例**

- 成功返回:

	``` 
	{
	    "status": 1,
	    "message": "",
	    "data": true
	}
	```
- 错误返回:
 
	```
	{
	    "status": -17,
	    "message": "业务员编号 : 188  ; entity已经存在"
	}
	```

**返回参数说明** 
- 见[基本参数说明](../../readme.md#api整体返回说明)

# 1.8 /trace

**简要描述：** 
- 获取指定业务员的百度鹰眼轨迹信息
- 原装调用百度接口的数据中转，具体接口说明见[gethistory——查询历史轨迹](http://lbsyun.baidu.com/index.php?title=yingyan/api/track#gethistory.E2.80.94.E2.80.94.E6.9F.A5.E8.AF.A2.E5.8E.86.E5.8F.B2.E8.BD.A8.E8.BF.B9)
- 起始时间根据06:00-22:00与上下班签到时间的交集确认

**请求URL：** 
- ` /api/lbs/salesman/trace `
  
**请求方式：**
- POST 

**参数：** 

|参数名|必选|类型|说明|
|:----|:---|:-----|-----|
|mid   |是|int |指定查询的业务员编号|
|date|否|string|默认为今日，支持"YYYY-MM-DD"，可被new Date()格式化即可|


- 实际使用百度map的api进行请求，举个栗子：
	- [http://api.map.baidu.com/trace/v2/track/gethistory?entity_name=188&start_time=1477173600&end_time=1477220027&process_option=need_mapmatch=1,transport_mode=2&is_processed=1&page_size=5000&sort_type=1&ak=Ql6WWdnoZGfbLEwtaMgHDKDX4w98haWO&service_id=126838](http://api.map.baidu.com/trace/v2/track/gethistory?entity_name=188&start_time=1477173600&end_time=1477220027&process_option=need_mapmatch=1,transport_mode=2&is_processed=1&page_size=5000&sort_type=1&ak=Ql6WWdnoZGfbLEwtaMgHDKDX4w98haWO&service_id=126838)

**返回示例**
- 具体接口说明见[gethistory——查询历史轨迹](http://lbsyun.baidu.com/index.php?title=yingyan/api/track#gethistory.E2.80.94.E2.80.94.E6.9F.A5.E8.AF.A2.E5.8E.86.E5.8F.B2.E8.BD.A8.E8.BF.B9)

**返回参数说明** 
- 具体接口说明见[gethistory——查询历史轨迹](http://lbsyun.baidu.com/index.php?title=yingyan/api/track#gethistory.E2.80.94.E2.80.94.E6.9F.A5.E8.AF.A2.E5.8E.86.E5.8F.B2.E8.BD.A8.E8.BF.B9)
