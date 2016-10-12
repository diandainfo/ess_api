
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
- 设置单个店铺的相关信息

**请求URL：** 
- ` /api/lbs/stores/set `
  
**请求方式：**
- POST 

**参数：** 

|参数名|必选|类型|说明|
|:----|:---|:-----|-----|
|sid   |是|int |storeId,店铺编号|
|lrb  |否|boolean  |locationResetBoolean,是否需要重新抓取定位地址和对应坐标，地址发生较大改变时需要置为true|
|user  |否|string  |店主姓名|
|title |否 |string |店铺名称|
|phone  |否|string |手机号|
|state  |否|int     |状态|
|vip  |否|int     |vip等级|
|cid  |否|int     |cityId，区县编号|
|address |否 |string     |地址|

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