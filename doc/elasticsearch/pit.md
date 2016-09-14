
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
