
# LBS - 店铺信息

# 1.1 /get

**简要描述：** 
- 获取单个商品的信息

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
||  |     ||
||  |     ||

