
# Search
- **搜索实现方案**
- 使用ES中Elasticsearch-ik+Elasticsearch-pinyin实现

## 1. Index
- **索引**
- #为注释信息

### 创建索引
```
{
    "index": {
        "analysis": {
            "analyzer": {					#创建分析器
                "only_pinyin_analyzer": {
                    "tokenizer": "keyword",		#使用标准keyword分词器
                    "filter": ["only_pinyin","lowercase","_pattern"]  #依次通过首字母拼音过滤，大写转小写，字符过滤
                }
                , "full_pinyin_analyzer": {
                    "tokenizer": "keyword",
                    "filter": ["full_pinyin","lowercase","_pattern"]  #依次通过全拼拼音过滤，大写转小写，字符过滤
                }
            },
            "tokenizer": {					#创建分词器
                "prefix_pinyin": {
                    "type": "pinyin",
                    "first_letter": "prefix",
                    "padding_char": ""
                }
            },
            "filter": {						#创建过滤器
                "_pattern":{					#字符过滤，使用正则过滤非英文(大小写)、数字字符
                    "type":"pattern_replace",		#使用类型为"pattern_replace"的预置过滤器
                    "pattern": "([\\W])",			#正则
                    "replacement": ""				#替换为""，即删除
                }
                ,"only_pinyin": {				#拼音过滤，提取拼音首字母;"百事可乐" » "bskl"
                    "type": "pinyin",
                    "first_letter": "only",
                    "padding_char": ""
                }
                , "full_pinyin": {				#拼音过滤，提取全拼;"百事可乐" » "baishikele"
                    "type": "pinyin",
                    "first_letter": "none",
                    "padding_char": ""
                }
            }
        }
    }
}
```

## 2. Mapping
- **映射**
- #为注释信息

### 创建映射
```
{
    "shelf_goods": {
        "properties": {
            ...												## 略去无关字段
            "name": {										#商品名称原始数据，不分词
                "type": "string",
                "index": "not_analyzed"
            },
            "name_ik": {									#商品名称，ik分词
                "type": "string",
                "analyzer": "ik_max_word",					#使用最大粒度分词
                "search_analyzer": "ik_max_word",
                "term_vector": "with_positions_offsets"		#保存分词位置	
            },
            "name_only_pinyin": {							#商品名称，pinyin首字母转换
                "type": "string",
                "analyzer": "only_pinyin_analyzer",			#分析器选用上文index中only_pinyin_analyzer
                "search_analyzer": "only_pinyin_analyzer"	#查询分析器
            },
            "name_full_pinyin": {							#商品名称，pinyin全拼转换
                "type": "string",
                "analyzer": "full_pinyin_analyzer",			#分析器选用上文index中full_pinyin_analyzer
                "search_analyzer": "full_pinyin_analyzer"	#查询分析器
            },
            ...												## 略去无关字段
			"cityId": {										#城市编号，不进行分词
                "type": "string",
                "index": "not_analyzed"
            },
            "fcn": {										#一级分类名称，不进行分词
                "type": "string",
                "index": "not_analyzed"
            },
            "scn": {										#二级分类名称，不进行分词
                "type": "string",
                "index": "not_analyzed"
            },
            "tags": {										#标签，不进行分词
                "type": "string",
                "index": "not_analyzed"
            },
            "onState": {									#商品状态，1为在售，0为下架
                "type": "integer"
            },
			...												## 略去无关字段
        }
    }
}
```

## 3. Suggest
- **搜索建议**
- 使用[Elasticsearch.js » search](https://www.elastic.co/guide/en/elasticsearch/client/javascript-api/current/api-reference.html#api-search)方法实现
- [Query DSL » Full text queries » Match Query » match_phrase_prefix](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-match-query.html#query-dsl-match-query-phrase-prefix)
- [Query DSL » Compound queries » Bool Query](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-bool-query.html#query-dsl-bool-query)
- //为注释信息
```
body: {
    from: 0,												//起始量
    size: 10,												//偏移量
    fields: ['name'],										//筛选字段，查询后只返回name字段
    query: {
        bool: {
            should: [
                {
                    match_phrase_prefix: {					//前缀匹配搜索
                        name_only_pinyin: data['keyword']	//使用拼音首字母匹配
                    }
                }, {
                    match_phrase_prefix: {
                        name_full_pinyin: data['keyword']	//使用拼音全拼匹配
                    }
                }
            ],
            minimum_should_match: 1,						//至少匹配一个查询
            must: [
                {
                    term: {
                        onState: 1							//onState字段匹配1，
                    }
                 }, {
                    term: {
                        cityId: data['cityId']				//cityId字段匹配对应城市编号
                    }
                }
            ]
        }
    }
}
```
- 将查询后结果进行重新构造，得到name字段的数组数据

## 4. Search
- **搜索关键词**
- 查询实现技术见上Suggest说明
- //为注释信息
```
body: {
    fields: ["name"],
    from: data['from'],
    size: data['size'],
    query: {
        bool: {
            should: [							
                {
                    term: {
                        fcn: data['keyword']		//一级分类
                    }
                }
                , {
                    term: {
                        scn: data['keyword'] 		//二级分类
                    }
                }
                , {
                    term: {
                        tags: data['keyword'] 		//标签化查询
                    }
                }
                , {
                    match_phrase: {			//## 改为完全匹配
                        name_ik: data['keyword'] 	//名称的中文分词
                    }
                }
            ]
			,minimum_should_match: 1 				//四个should查询，至少命中一个
            ,must: [
                {
                    term: {
                        onState: 1
                    }
                }, {
                    term: {
                        cityId: data['cityId'] 		//城市编号
                    }
                }
            ]
        }
    },
    highlight: { 									//命中词高亮
        pre_tags: ["<tags>"],
        post_tags: ["</tags>"],
        fields: {
            name_ik: {}
        }
    }
}
```
- 将查询结果进行重新构造，将_score加入返回信息内
