
# stts_api

stts服务的api文档

## API接口列表(使用层次递进的router结构)
* /api
  * /log 日志
    * /history 浏览记录 [ 日志 - 浏览记录 ](./log/history/readme.md)
      * /goods 商品浏览记录 
        * POST : /set 写入 [ 日志 - 浏览记录 - 写入](./log/history/readme.md#11-goodsset)
        * POST : /get 获取 [ 日志 - 浏览记录 - 获取](./log/history/readme.md#12-goodsget)
    * /error 错误日志 [ 日志 - 错误日志 ](./log/error/readme.md)
      * /back 后台错误日志
        * POST : /set 写入 [ 日志 - 错误日志 - 写入](./log/error/readme.md#11-backset)
        * GET  : /get 获取 [*不提供对外接口，使用`/log/error`查看页面*]
    * /info 信息日志 [ 日志 - 信息日志 ](./log/info/readme.md)
      * /back 后台信息日志
        * POST : /set 写入 [ 日志 - 信息日志 - 写入](./log/info/readme.md#11-backset)
        * GET  : /get 获取 [*不提供对外接口，使用`/log/info`查看页面*]


## ElasticSearch
* [ElasticSearch Instruction](./doc/elasticsearch/readme.md)
* [ElasticSearch Installation](./doc/elasticsearch/installation.md)
* [ElasticSearch Response](./doc/elasticsearch/response.md)
* [ElasticSearch Update Log](./doc/elasticsearch/update.md)
