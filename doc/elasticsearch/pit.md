
# Pit

**Elasticsearch中的坑**

## 1. Deleted documents show up in completion suggester
**当在index中删除document后，依然可以通过completion suggester查询到对应的信息**
### 1.1 document
* [Completion Suggester](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-suggesters-completion.html)

### 1.2 bug
* Github中Elasticsearch的issue
  * [Deleted documents show up in completion suggester #7761](https://github.com/elastic/elasticsearch/issues/7761)
* Stack Overflow:
  * [elasticsearch completion field not deleting](http://stackoverflow.com/questions/27074593/elasticsearch-completion-field-not-deleting)

### 1.3 ~~solve~~
* [Completion Suggester Version 2](https://github.com/elastic/elasticsearch/issues/8909)
* [Add support for returning documents with completion suggester #19536](https://github.com/elastic/elasticsearch/pull/19536)

# wait for v5.0

***
