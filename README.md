
# ess_api

**基于ES的上层service的api文档**

## API整体返回说明
- 返回参数中，一般包含以下参数
  
|参数名|必有|类型|说明|取值范围及说明|
|:----|:---|:---|:-----|--:|
|status|是|int|判断是否请求成功，及内部逻辑是否完成|>0即为成功，<0即为失败;仅代表请求完成并且成功，并不代表取值不为空,status>0值，data可能为空|
|message|是|string|存放错误信息|status>0时，此值为''，无意义。使用ES数据时，此值可能出现大量ES错误，建议不要使用此值进行页面显示|
|data|是|不定|返回数据|可能取值 int、object、array，视具体请求进行分别拆分|
|count|否|int|总计符合条件的结果条数|只有需要分页的数据才会返回此值|



## API接口列表(使用层次递进的router结构)
* /api
  * /log 日志
    * /history 浏览记录 [ 日志 - 浏览记录 ](./log/history/readme.md)
      * /goods 商品浏览记录 
        * /set : POST [ 日志 - 浏览记录 - 写入](./log/history/readme.md#11-goodsset)
        * /get : POST [ 日志 - 浏览记录 - 获取](./log/history/readme.md#12-goodsget)
    * /error 错误日志 [ 日志 - 错误日志 ](./log/error/readme.md)
      * /back 后台错误日志
        * /set : POST [ 日志 - 错误日志 - 写入](./log/error/readme.md#11-backset)
        * /get : GET 获取 [*不提供对外接口，使用`/log/error`查看页面*]
    * /info 信息日志 [ 日志 - 信息日志 ](./log/info/readme.md)
      * /back 后台信息日志
        * /set : POST [ 日志 - 信息日志 - 写入](./log/info/readme.md#11-backset)
        * /get : GET 获取 [*不提供对外接口，使用`/log/info`查看页面*]
    * /search 搜索关键词日志 [ 日志 - 搜索关键词日志 ](./log/search/readme.md)
      * /get  : POST [ 日志 - 搜索关键词 - 获取用户搜索历史](./log/search/readme.md#11-get) 
      * /goods: POST [ 日志 - 搜索关键词 - 关键词聚合](./log/search/readme.md#12-goods) 
  * /goods 商品
  	* /shelf [货架商品](./goods/shelf/readme.md)
  	  * /suggest : POST [货架商品 - 搜索建议](./goods/shelf/readme.md#11-suggest)  
  	  * /search : POST  [货架商品 - 搜索关键词](./goods/shelf/readme.md#12-search)  
  	  * /analyze : POST  [货架商品 - 分词](./goods/shelf/readme.md#13-analyze)  
  * /recommend 推荐
	* /goods [商品](./recommend/goods/list/readme.md)
	  * /list :POST [推荐 - 商品 - 列表](./recommend/goods/list/readme.md#11-list)
  * /lbs LBS相关接口
    * /stores [店铺](./lbs/stores/readme.md)
      * /get : POST [LBS - 获取单个店铺信息](./lbs/stores/readme.md#11-get)
      * /set : POST [LBS - 设置单个店铺信息](./lbs/stores/readme.md#12-set)
      * /list : POST [LBS - 筛选、获取店铺列表](./lbs/stores/readme.md#13-list)
      * /distance : POST [LBS - 获取单个店铺的定位距离](./lbs/stores/readme.md#14-distance) 
	   
## [ElasticSearch](./doc/elasticsearch/readme.md)
* [ElasticSearch Instruction](./doc/elasticsearch/instruction.md)
* [ElasticSearch Pit](./doc/elasticsearch/pit.md)
* [ElasticSearch Installation](./doc/elasticsearch/installation.md)
* [ElasticSearch Option](./doc/elasticsearch/option.md)
* [ElasticSearch Response](./doc/elasticsearch/response.md)
* [ElasticSearch Update](http://116.228.89.150:8090/pages/viewpage.action?pageId=4620450)

## Kibana
* [Kibana Installation](./doc/kibana/installation.md)

# [LICENSE](./LICENSE)
