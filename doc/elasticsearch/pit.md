
# Pit

**Elasticsearch中的坑**

## 1. Deleted documents show up in completion suggester
**当在index中删除document后，依然可以通过completion suggester查询到对应的信息**
### 1.1 Document
* [Completion Suggester](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-suggesters-completion.html)

### 1.2 Bug
* github中Elasticsearch的issue[Deleted documents show up in completion suggester](https://github.com/elastic/elasticsearch/issues/7761)
* stackoverflow中[elasticsearch completion field not deleting](http://stackoverflow.com/questions/27074593/elasticsearch-completion-field-not-deleting)

### 1.9 Solve
* [Completion Suggester Version 2](https://github.com/elastic/elasticsearch/issues/8909)
* 换用search实现，参见[搜索具体实现](./search/readme.md)


## ~~2. 数据类型设置为Date，该字段值为空时报错~~
* es_v2.3.4 无此问题

## ~~3. Elasticsearch-jdbc，使用普通用户权限启动.sh &后，无法使用ssh登录~~
* 应是linux系统设置原因，可能跟内存、cpu占用有关

## 4. 使用search查询进行分页时，无法查询from 10000以后的数据

### 4.2 Bug
```
{ [Error: [query_phase_execution_exception] Result window is too large, from + size must be less than or equal to: [10000] but was [278055]. See the scroll api for a more efficient way to request large data sets. This limit can be set by changing the [index.max_result_window] index level parameter.]
  status: 500,
  displayName: 'InternalServerError',
  message: '[query_phase_execution_exception] Result window is too large, from + size must be less than or equal to: [10000] but was [278055]. See the scroll api for a more efficient way to request large data sets. This limit can be set by changing the [index.max_result_window] index level parameter.',
  path: '/orders_goods/goods_details/_search',
  query: {},
  body: '{"from":"278040","size":"15","sort":[{"oid":"desc"}],"query":{"range":{"createdAt":{"gte":"2016-09-01","lte":"2016-09-14"}}}}',
  statusCode: 500,
  response: '{"error":{"root_cause":[{"type":"query_phase_execution_exception","reason":"Result window is too large, from + size must be less than or equal to: [10000] but was [278055]. See the scroll api for a more efficient way to request large data sets. This limit can be set by changing the [index.max_result_window] index level parameter."}],"type":"search_phase_execution_exception","reason":"all shards failed","phase":"query_fetch","grouped":true,"failed_shards":[{"shard":0,"index":"orders_goods_v1","node":"TH2v80c7TmyWaVk7NCUbMQ","reason":{"type":"query_phase_execution_exception","reason":"Result window is too large, from + size must be less than or equal to: [10000] but was [278055]. See the scroll api for a more efficient way to request large data sets. This limit can be set by changing the [index.max_result_window] index level parameter."}}]},"status":500}',
  toString: [Function],
  toJSON: [Function] }
```
### 4.3 Reference
* [Elasticsearch 2.1: Result window is too large (index.max_result_window)](http://stackoverflow.com/questions/35206409/elasticsearch-2-1-result-window-is-too-large-index-max-result-window)

## 5. geo_point相关
- 5.1 其中存储的坐标数据，latitude(纬度)在前，longitude(经度)在后，而不汉语中传统的“经纬度”序列。

  - 具体查看document [Geo-point datatype](https://www.elastic.co/guide/en/elasticsearch/reference/current/geo-point.html)
  - 第4个栗子
    ```
      PUT my_index/my_type/4
      {
        "text": "Geo-point as an array",
        "location": [ -71.34, 41.12 ] 
      }
      // Geo-point expressed as an array with the format: [ lon, lat]
    ```
    
- 5.2 根据distance筛选points时，在results中显示distance
  
  - 使用sort排序即可获知具体的distance信息，[Return distance in elasticsearch results?](http://stackoverflow.com/a/25522956/3214134)
  - 使用`script_fields`来进行distance的运算,[Return distance in elasticsearch results?](http://stackoverflow.com/a/9309674/3214134)
  ```
      curl -XGET 'http://127.0.0.1:9200/geonames/_search?pretty=1'  -d '
    {
       "fields" : [ "_source" ],
       "script_fields" : {
          "distance" : {
             "params" : {
                "lat" : 2.27,
                "lon" : 50.3
             },
             "script" : "doc[\u0027location\u0027].distanceInKm(lat,lon)"
          }
       }
    }'
  ```
    - 使用script_fields+script时报错，`Elasticsearch failed to run inline script [doc…] using lang groovy`
      - 某个值不存在 [Elasticsearch failed to run inline script [doc…] using lang groovy](http://stackoverflow.com/a/37836674/3214134)
    - 或`"caused_by":{"type":"missing_method_exception","reason":...`
      - 无此方法 
