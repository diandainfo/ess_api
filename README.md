
# ess_api

**基于ES的上层service的api文档**

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
	* /goods [商品](./recommend/goods/readme.md)
	  * /list :POST [推荐 - 商品 - 列表](./recommend/goods/readme.md#11-list)
  * /lbs LBS相关接口
    * /stores [店铺](./lbs/stores/readme.md)
      * /get : POST [LBS - 获取单个店铺信息](./lbs/stores/readme.md#11-get)
      * /set : POST [LBS - 设置单个店铺信息](./lbs/stores/readme.md#12-set)
      * /list : POST [LBS - 筛选、获取店铺列表](./lbs/stores/readme.md#13-list) 
	   
## [ElasticSearch](./doc/elasticsearch/readme.md)
* [ElasticSearch Instruction](./doc/elasticsearch/instruction.md)
* [ElasticSearch Pit](./doc/elasticsearch/pit.md)
* [ElasticSearch Installation](./doc/elasticsearch/installation.md)
* [ElasticSearch Option](./doc/elasticsearch/option.md)
* [ElasticSearch Response](./doc/elasticsearch/response.md)
* [ElasticSearch Update](./doc/elasticsearch/update.md)

## Kibana
* [Kibana Installation](./doc/kibana/installation.md)

# [History](./history.md)

# [LICENSE](./LICENSE)
