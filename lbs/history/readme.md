
# LBS - 历史信息

# 0 Mapping
**具体数据类型见下 `type` 列:**
```
location_history: {
        properties: {
            mid: {type: 'integer'}       // salesmanEntityId,业务员实体编号
            , cid: {type: 'integer'}    //城市编号
            // 记录类型 0:当日总计 1:上班签到 2:店铺签到 3:移动轨迹 4:停留点 5:失联轨迹 6:下班签退 7:分时统计
            , type: {type: 'integer'}
            , date: {type: 'date', format: 'yyyy-MM-dd'}    //日期
            // 起始时间,非延时性操作(1\2\4)仅有起始时间,使用标准ms级时间戳,只在于百度api接驳时进行s级时间戳转换
            , startAt: {type: 'long'}
            // 截止时间,type=0时，记录今日已分析轨迹点的截止时间
            , endAt: {type: 'long'}   
            , sid: {type: 'integer'}    //type=2时，签到的店铺编号
            , sna: {type: 'string', index: 'not_analyzed'} //type=2时，店铺名称
            , lat: {type: 'double'}  //定位坐标 - 纬度
            , lon: {type: 'double'}  //定位坐标 - 经度
            , la: {type: 'string', index: 'not_analyzed'} // 定位地址
            , ld: {type: 'double'}  // 定位距离、定位误差
            , distance: {type: 'double'} //type=3时，移动距离
            , duration: {type: 'integer'} //持续时间，单位(s)，type=0时，为数组
        }
    }
```

# 1.1 /sign

**简要描述：** 
- 写入 上、下班签到、退 信息

**请求URL：** 
- ` /api/lbs/history/sign `
  
**请求方式：**
- POST 

**参数：** 

|参数名|必选|类型|说明|
|:----|:---|:-----|-----|
|mid   |是|int |业务员编号|
|cid|是|int|业务员所在城市编号|
|type|否|int|默认为1,可选值为(1\|6);1为上班签到，6为下班签退;签到类型见上Mapping|
|lat|否|double|纬度，签到时所在坐标点纬度|
|lon|否|double|经度，签到时所在坐标点经度|
|time|否|long|13位，签到时间戳，默认为当前时刻|

**返回示例**

- 请求：

	```
	 /api/lbs/history/sign -d '
	{
		type:1
		,mid:188
		,cid:320400
		,lat:31.682951
		,lon:119.93631
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

**返回参数说明** 

|参数名|类型|说明|
|:----|:---|:-----|
|status|int|是否签到成功，>0即为成功，<0即为失败|
|data|int|是否签到成功，true为成功，失败时message有信息，若无则说明ES内部发生错误，查看ess_log即可|

# 1.2 /check

**简要描述：** 
- 写入 店铺签到

**请求URL：** 
- ` /api/lbs/history/check `
  
**请求方式：**
- POST 

**参数：** 

|参数名|必选|类型|说明|
|:----|:---|:-----|-----|
|mid   |是|int |业务员编号|
|sid|是|int|需要签到的店铺编号|
|sna|是|string|需要签到的店铺名称|
|cid|是|int|业务员所在城市编号|
|lat|是|double|纬度，签到时所在坐标点纬度|
|lon|是|double|经度，签到时所在坐标点经度|
|address|是|string|签到时定位地址|
|distance|是|double|定位距离，通过`/api/lbs/stores/distance`获取的签到定位距离|
|ld|否|double|定位精度，签到点的定位精度，用于判断定位误差|
|time|否|long|13位，签到时间戳，默认为当前时刻|

**返回示例**

- 请求：

	```
	 /api/lbs/history/check -d '
	{
		mid:188
		,cid:320400
		,sid:197
		,sna:"xxxx店"
		,distance:51
		,lat:31.682951
		,ld:1.214
		,address:'xx省xx市xx路xx号'
		,lon:119.93631
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

**返回参数说明** 

|参数名|类型|说明|
|:----|:---|:-----|
|status|int|是否签到成功，>0即为成功，<0即为失败|
|data|int|是否签到成功，true为成功，失败时message有信息，若无则说明ES内部发生错误，查看ess_log即可|


# 1.3 /get

**简要描述：** 
- 获取指定业务员的上班签到、下班签退、店铺签到记录

**请求URL：** 
- ` /api/lbs/history/get `
  
**请求方式：**
- POST 

**参数：** 

|参数名|必选|类型|说明|
|:----|:---|:-----|-----|
|mid   |是|int |业务员编号|
|date|否|string|默认为今日，支持"YYYY-MM-DD"，可被new Date()格式化即可|

**返回示例**

- 请求：

	```
	 /api/lbs/history/get -d '
	{
		mid:188
		,date:'2016-10-22'
	}'
	```
- 时，返回

	``` 
	{
	    "status": 1,
	    "message": "",
	    "data": [
	        {
	            "mid": 188,
	            "cid": 320400,
	            "type": 1,
	            "lat": 31.682951,
	            "lon": 119.93631,
	            "startAt": 1477092600000,
	            "date": "2016-10-22"
	        },
	        {
	            "type": 2,
	            "mid": 188,
	            "sid": 197,
	            "sna": "xxxxx",
	            "cid": 320400,
	            "lat": 31.682951,
	            "lon": 119.93631,
	            "ld": 51,
	            "startAt": 1477096200000,
	            "date": "2016-10-22"
	        }
	    ]
	}
	```

**返回参数说明** 

|参数名|类型|说明|
|:----|:---|:-----|
|status|int|是否签到成功，>0即为成功，<0即为失败|
|data|array|具体参数说明见上	[mapping](0-mapping)	|

# 1.4 /salesman

**简要描述：** 
- 获取指定业务员的单日出勤情况
- 不存在则无返回

**请求URL：** 
- ` /api/lbs/history/salesman `
  
**请求方式：**
- POST 

**参数：** 

|参数名|必选|类型|说明|
|:----|:---|:-----|-----|
|mid   |是|int/array |业务员编号，数组或者int|
|date|否|string|默认为今日，支持"YYYY-MM-DD"，可被new Date()格式化即可|

**返回示例**

- 请求：
  ```
  /api/lbs/history/salesman -d '
  {
    mid:[123,69,140]
    ,date:'2016-12-11'
  }'
  ```

- 时，返回
  - **123不存在记录，则返回对象中缺失该key**
  ``` 
  {
    "status": 1,
    "message": "",
    "data": {
        "69": {
            "move": "15.50",
            "stop": 1042,
            "offline": 0
        },
        "140": {
            "move": "16.20",
            "stop": 1045,
            "offline": 0
        }
    }
  }
  ```

**返回参数说明** 

|参数名||类型|说明|
|:----|:--|:---|:-----|
|status||int|是否签到成功，>0即为成功，<0即为失败|
|data||object|||
||move|float|移动距离，单位km|
||stop|int|停留时间，单位s|
||offline|int|失联时间，单位s|
