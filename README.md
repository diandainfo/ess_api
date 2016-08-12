
# ess_api

**基于ES的上层service的api文档**

## API接口列表(使用层次递进的router结构)
* /api
  * /log 日志
    * /history 浏览记录 [ 日志 - 浏览记录 ](./log/history/readme.md)
      * /goods 商品浏览记录 
        * /set : POST 写入 [ 日志 - 浏览记录 - 写入](./log/history/readme.md#11-goodsset)
        * /get : POST 获取 [ 日志 - 浏览记录 - 获取](./log/history/readme.md#12-goodsget)
    * /error 错误日志 [ 日志 - 错误日志 ](./log/error/readme.md)
      * /back 后台错误日志
        * /set : POST 写入 [ 日志 - 错误日志 - 写入](./log/error/readme.md#11-backset)
        * /get : GET 获取 [*不提供对外接口，使用`/log/error`查看页面*]
    * /info 信息日志 [ 日志 - 信息日志 ](./log/info/readme.md)
      * /back 后台信息日志
        * /set : POST 写入 [ 日志 - 信息日志 - 写入](./log/info/readme.md#11-backset)
        * /get : GET 获取 [*不提供对外接口，使用`/log/info`查看页面*]
  * /goods 商品
  	* /shelf 货架商品 [货架商品](./goods/shelf/readme.md)
  		* /suggest : POST 搜索建议 [货架商品 - 搜索建议](./goods/shelf/readme.md#11-suggest)  
  		* /search : POST 搜索关键词 [货架商品 - 搜索关键词](./goods/shelf/readme.md#11-search)  

## ElasticSearch
* [ElasticSearch Instruction](./doc/elasticsearch/readme.md)
* [ElasticSearch Installation](./doc/elasticsearch/installation.md)
* [ElasticSearch Response](./doc/elasticsearch/response.md)
* [ElasticSearch Update](./doc/elasticsearch/update.md)
