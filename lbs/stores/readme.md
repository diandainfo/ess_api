
# LBS - 店铺信息

# 1.1 /get

**简要描述：** 
- 获取单个店铺的相关信息

**请求URL：** 
- ` /api/lbs/stores/get `
  
**请求方式：**
- POST 

**参数：** 

|参数名|必选|类型|说明|
|:----|:---|:-----|-----|
|sid   |是|int |storeId,店铺编号|

**返回示例**

- 请求：

	```
	 /api/lbs/stores/get -d '
	{
		sid:1
	}'
	```
- 时，返回

	``` 
	 {
	    "status": 1,
	    "message": "",
	    "data": {
	        "sid": "1",
	        "user": "1",
	        "title": "xxxxxx",
	        "phone": "01234567891",
	        "address": "xx省xx市xx区xx路xx号",
	        "state": "1",
	        "vip": "4",
	        "cid": 320411,
	        "createdAt": 1407247068000,
	        "updatedAt": 1476265138803,
	        "formatted_address": "xx省xx市xx区xx路xx号",
	        "province": "xx省",
	        "city": "xx市",
	        "district": "xx区",
	        "street": [],
	        "location": {
	            "lat": 31.818798,
	            "lon": 119.970018
	        }
	    }
	}
	```

**返回参数说明** 

|参数名||类型|说明|
|:----|:---|:---|:-----|
|data||||
||sid  |int     |店铺编号|
||user  |string  |店主姓名|
||title  |string |店铺名称|
||phone  |string |手机号|
||state  |int     |状态|
||vip  |int     |vip等级|
||cid  |int     |cityId，区县编号|
||address  |string     |地址|
||createdAt  |long     |创建时间|
||updatedAT  |long     |更新时间|
||formatted_address  |string     |格式化后地址，定位地址（根据此地址进行坐标抓取）|
||location  |object     |地理坐标|
||province  |string     |省份|
||city  |string     |地级市|
||district  |string     |区县|
||street  |string     |街道|
||precise  |float     |定位精度(忽略即可)|

# 1.2 /set

**简要描述：** 
- 更新单个店铺的相关信息
- 更新店铺关联业务员的编号
- 有则更新，无则新建

**请求URL：** 
- ` /api/lbs/stores/set `
  
**请求方式：**
- POST 

**参数：** 

|参数名|必选|类型|说明|
|:----|:---|:-----|-----|
|sid   |是|int |storeId,店铺编号|
|lat|否|double|**新增店铺时必须传值**,latitude,百度定位坐标、纬度数据|
|lon|否|double|**新增店铺时必须传值**,longitude,百度定位坐标、经度数据|
|fd|否|string|**新增店铺时必须传值**,formatted_address,百度定位标准化地址|
|user  |否|string  |店主姓名|
|title |否 |string |店铺名称|
|phone  |否|string |手机号|
|state  |否|int     |状态|
|vip  |否|int     |vip等级|
|cid  |否|int     |countyId，区县编号|
|address |否 |string     |地址|
|mid|否|int|关联业务员编号|

**返回示例**

- 成功返回:

	``` 
	{
	    "status": 1,
	    "message": "",
	    "data": true
	}
	```
- 错误信息:
	
	```
	{
   		"status": -11,
    	"message": "未获取到店铺编号"
	}
	```

**返回参数说明** 

|参数名|类型|说明|
|:----|:---|:-----|
|data|boolean|更新结果，true为成功，false为部分节点失败|
|status|int|为负值，说明更新失败，message为失败原因|

# 1.3 /list

**简要描述：** 
- 根据条件筛选、获取某些店铺的相关信息

**请求URL：** 
- ` /api/lbs/stores/list `
  
**请求方式：**
- POST 

**参数：** 

|参数名|必选|类型|默认值|说明|
|:----|:---|:-----|--:|-----|
|lat|是|float|-|当前定位点纬度值|
|lon|是|float|-|当前定位点经度值|
|mid|是|int|-|业务员编号|
|from|否|int|0|单位(m)，起始距离值，即从定位点开始的`from`距离外|
|to|否|int|5000|单位(m),终止距离值，即从定位点到`to`的距离内，和`from`结合组成圆环区域|
|offset|否|int|0|起始量|
|limit|否|int|20|偏移量|
|st|否|int|-|店铺类型，可取值（1-5、99），必须是数组,[1]或[1,2]|
|sc|否|int|-|店铺大小，可取值（10、20、30），必须是数组|
|sv|否|int|-|店铺等级，可取值（1-4），必须是数组|
|key|否|string|-|搜索关键词，可填写：店铺名称、店主姓名、手机号|

**返回示例**

- 给定一个坐标点和业务员编号，例如:
 	
	```
	{
      mid:188
	  ,lat: 31.682951
	  ,lon: 119.93631
	}
    ```
- 若周围有相应店铺，则成功返回:

	``` 
	 {
	    "status": 1,
	    "message": "",
	    "data": {
	        "sid": "1",
	        "user": "1",
	        "title": "xxxxxx",
	        "phone": "01234567891",
	        "address": "xx省xx市xx区xx路xx号",
	        "state": "1",
	        "vip": "4",
	        "cid": 320411,
	        "createdAt": 1407247068000,
	        "updatedAt": 1476265138803,
	        "formatted_address": "xx省xx市xx区xx路xx号",
	        "province": "xx省",
	        "city": "xx市",
	        "district": "xx区",
	        "street": [],
	        "location": {
	            "lat": 31.818798,
	            "lon": 119.970018
	        },
			"distance": 0
	    },
		 "count": 331
	}
	```
- 否则返回:

	```
	{
	    "status": 1,
	    "message": "",
	    "data": [],
	    "count": 0
	}
	```
- 错误信息，具体错误信息记录在项目error_log中:
	
	```
	{
   		"status": -1
	}
	```

**返回参数说明** 

|参数名||类型|说明|
|:----|:---|:---|:-----|
|count||int|总计符合条件的店铺搜索结果条数|
|data|   |     ||
||distance  |int     |距定位点距离，单位为m|
||-  |-     |其他数据参见单个店铺的返回说明|


# 1.4 /sync

**简要描述：** 
- 根据条件筛选、获取某些店铺的相关信息

**请求URL：** 
- ` /api/lbs/stores/list `
  
**请求方式：**
- POST 

**参数：** 

|参数名|必选|类型|默认值|说明|
|:----|:---|:-----|--:|-----|
|offset|否|int|0|起始量|
|limit|否|int|10000|偏移量，最大值为10000，单次查询数量不得超过10000|
|sids   |是|int/array |指定设置店铺信息的店铺编号,支持输入多个sid，使用数组，如： [1,2,3]|

**返回示例**

- 给定一个坐标点和业务员编号，例如:
 	
	```
	{
      offset:0
	  ,limit:1
	}
    ```
- 成功返回:

	``` 
	 {
	    "status": 1,
	    "message": "",
	    "count": 10,
	    "data": [
	        {
	            "sid": 4,
	            "user": "4",
	            "title": "xxx",
	            "phone": "12345678",
	            "address": "xxxxx",
	            "state": -10,
	            "vip": 1,
	            "cid": 320507,
	            "createdAt": 1407247068000,
	            "updatedAt": 1477357391876,
	            "capacity": 0,
	            "type": 0,
	            "mid": 0,
	            "formatted_address": "xxxx",
	            "precise": 0.16,
	            "province": "xxx",
	            "city": "xxxx",
	            "district": "xxxx",
	            "street": "",
	            "lat": 31.939946043961303,
	            "lon": 119.90315390841334
	        }
	    ]
	}
	```
- 错误信息，具体错误信息记录在项目error_log中:
	
	```
	{
   		"status": -1
	}
	```

**返回参数说明** 

|参数名||类型|说明|
|:----|:---|:---|:-----|
|count||int|总计符合条件的店铺搜索结果条数|
|data|   |     ||
||-  |-     |数据说明参见单个店铺的返回说明|


# 1.5 /batch

**简要描述：** 
- 批量设置店铺信息（vip、mid）

**请求URL：** 
- ` /api/lbs/stores/batch `
  
**请求方式：**
- POST 

**参数：** 

|参数名|必选|类型|说明|
|:----|:---|:-----|-----|
|sids   |是|int/array |指定设置店铺信息的店铺编号,支持输入多个sid，使用数组，如： [1,2,3]|
||否||以下参数必填一个，否则报错|
|vip|否|int|店铺的等级|
|mid|否|int|店铺的关联业务员编号|

**返回示例**

- 成功返回:

	``` 
	{
	    "status": 1,
	    "message": "",
	    "data": {
	        "total": 3,
	        "success": 2,
	        "failed": 1,
	        "results": [
	            {
	                "sid": "22221",
	                "message": "更新店铺出错 : document_missing_exception"
	            }
	        ]
	    }
	}
	```

**返回参数说明** 

|参数名||类型|说明|
|:----|:---|:---|:-----|
|data|   |     ||
||total|int|总计需要设置的sid数量|
||success|int|成功的数量|
||failed|int|失败的数量|
||results|array|失败的原因|



# 1.7 /distance

**简要描述：** 
- 根据定位坐标计算距离某个店铺的距离
- 用于店铺签到的距离确认

**请求URL：** 
- ` /api/lbs/stores/distance `
  
**请求方式：**
- POST 

**参数：** 

|参数名|必选|类型|说明|
|:----|:---|:-----|-----|
|lat|是|float|当前定位点纬度值|
|lon|是|float|当前定位点经度值|
|sid|是|int|需要定位的店铺编号|



**返回示例**

  - 给定一个坐标点，和店铺编号，若店铺存在，则成功返回:

	``` 
	 {
	    "status": 1,
	    "message": "",
	    "data": 16236.643336986202
	}
	```
  - 店铺不存在，则返回:

	```
	{
	    "status": 1,
	    "message": "",
	    "data": -2
	}
	```
  - 错误信息:
	
	```
	{
	    "status": -1,
	    "message": "发生了一个错误"
	}
	```

**返回参数说明** 

|参数名|类型|说明|
|:----|:---|:---|
|data|float|定位坐标点距离店铺的距离，单位(m)|


# 1.8 /distances

**简要描述：** 
- 根据定位坐标批量计算多个店铺的距离
- 根据存在的店铺进行计算，返回结构已去除不存在的店铺编号

**请求URL：** 
- ` /api/lbs/stores/distances `
  
**请求方式：**
- POST 

**参数：** 

|参数名|必选|类型|说明|
|:----|:---|:-----|-----|
|lat|是|float|当前定位点纬度值|
|lon|是|float|当前定位点经度值|
|sid|是|int|必须是json格式的数组，不是String！需要计算距离的店铺编号|



**返回示例**

  - 给定一个坐标点，和店铺编号数组，成功返回:

	``` 
	{
	    "status": 1,
	    "message": "",
	    "count": 4,
	    "data": {
	        "1": 15441.248578589237,
	        "3": 16772.378726102823,
	        "4": 16772.378726102823
	    }
	}
	```
  - 店铺全部不存在，则返回:

	```
	{
	    "status": 1,
	    "message": "",
	    "count": 0,
	    "data": {}
	}
	```
  - 错误信息:
	
	```
	{
	    "status": -1,
	    "message": "发生了一个错误"
	}
	```

**返回参数说明** 

|参数名|类型|说明|
|:----|:---|:---|
|count|int|店铺编号确定存在的店铺数量|
|data|array + float|数组，key为店铺编号，value为坐标点距离店铺的距离，单位(m)|

