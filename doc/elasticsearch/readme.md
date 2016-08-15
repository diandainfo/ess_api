
# Elasticsearch

### 1.0 Instruction
|组件||版本|说明|状态|坑|
|:----|:---|:-----|-----|-----|-----|
|Elasticsearch |  |2.3.4 |使用elsearch用户运行|done|[《填坑指南》](./pit.md)|
|plugin |[Elasticsearch-head](https://github.com/mobz/elasticsearch-head)  |2.3.4 |UI|done||
||[Elasticsearch-analysis-pinyin](https://github.com/medcl/elasticsearch-analysis-pinyin)|1.9.4|pinyin转换器|done||
||[Elasticsearch-analysis-ik](https://github.com/medcl/elasticsearch-analysis-ik)|1.7.4|ik分词器|done||
||[Elasticsearch-deletebyquery](https://github.com/dermidgen/elasticsearch-deletebyquery)|None|批量查询删除插件|done|[需要安装plugin](http://stackoverflow.com/questions/37177359/how-to-delete-document-matching-a-query-using-official-elasticsearch-nodejs-clie)|
|river|[elasticsearch-jdbc](https://github.com/jprante/elasticsearch-jdbc)|2.3.4.0|es-river-jdbc|done|1、跟river有区别，具体参数见[Parameters](https://github.com/jprante/elasticsearch-jdbc#parameters) 2、[必须跑在jre8以上](https://github.com/jprante/elasticsearch-jdbc/issues/805)|
|Sheild||2.0+|[访问控制](https://www.elastic.co/downloads/shield)|||
|kibana|||UI|||

### 1.1 Installation
* [Installation Steps](https://www.elastic.co/downloads/elasticsearch)

### 1.2 Run
* **不建议**run在root下[bin/elasticsearch -Des.insecure.allow.root=true](http://stackoverflow.com/questions/34920801/how-to-run-elasticsearch-2-1-1-as-root-user-in-linux-machine)
* 创建一个非root用户用来run es
  * 后台运行es，需要使用ps+kill关闭
```
/bin/elasticsearch -d
```

### 1.3 Plugins
* [elasticsearch-head](https://github.com/mobz/elasticsearch-head#installing-and-running)
* analysis-ik & analysis-pinyin
 * [analysis-ik-v1.3.0](./analysis-ik/installation.md)
 * analysis-ik-v1.9.4和analysis-pinyin-v1.7.4下载对应release中的zip，解压，重启es即可
* elasticsearch-jdbc 下载对应release中zip，unzip，修改bin下.sh文件中响应的参数，shell启动即可
 * 【】中为说明，需要换成对应参数或者code
 * # 为注释，删除即可
 * 参数说明见[Parameters](https://github.com/jprante/elasticsearch-jdbc#parameters)
```
{
    "type": "jdbc",
    "jdbc": {
        "url": "jdbc:mysql://127.0.0.1:6335/test",     #mysql地址:端口/数据库名
        "user": "user",                                #mysql用户名
        "password": "123456",                          #用户名对应密码
        "sql": {
            "statement": "SELECT 【此处为查询语句】 WHERE 【判断语句】 > DATE_SUB(?,INTERVAL 1 MINUTE)"   # ? 为形参参数
              , "parameter": ["$metrics.lastexecutionstart"]  # 实参传递
        },                                             #SQL timestamp of the time when last execution started
        "index" : "test",                              #es的索引名
        "type" : "shelf_goods",                        #es索引下类型
        "timezone":"+08:00",                           #时区
        "max_bulk_volume":"100m",                      #允许批量请求的最大请求体大小，100Mb
        "metrics": {
            "enabled" : true                           #使用log日志记录器
        },
        "elasticsearch" : {
             "cluster" : "es2.3.4",               #Elasticsearch cluster name,应为/es/config下elasticsearch.yml中cluster.name
             "host" : "127.0.0.1",                #array of Elasticsearch host specifications (host name or host:port)
             "port" : 9300                        #port of Elasticsearch host,应为/es/config下elasticsearch.yml中transport.tcp.port
        }
    }
}
```
* Elasticsearch-deletebyquery插件安装
```
/bin/plugin install delete-by-query
```
