
# 货架商品

# 0.1 Index
```
{
    "index": {
        "analysis": {
            "analyzer": {
                "only_pinyin_analyzer": {
                    "tokenizer": "keyword",
                    "filter": ["only_pinyin","lowercase","_pattern"]
                }
                , "full_pinyin_analyzer": {
                    "tokenizer": "keyword",
                    "filter": ["full_pinyin","lowercase","_pattern"]
                }
            },
            "tokenizer": {
                "prefix_pinyin": {
                    "type": "pinyin",
                    "first_letter": "prefix",
                    "padding_char": ""
                }
            },
            "filter": {
                "_pattern":{
                    "type":"pattern_replace",
                    "pattern": "([\\W])",
                    "replacement": ""
                }
                ,"only_pinyin": {
                    "type": "pinyin",
                    "first_letter": "only",
                    "padding_char": ""
                }
                , "full_pinyin": {
                    "type": "pinyin",
                    "first_letter": "none",
                    "padding_char": ""
                }
            }
        }
    }
}
```

# 0.2 Mapping
```
{
    "shelf_goods": {
        "properties": {
            "id": {
                "type": "integer"
            },
            "name": {
                "type": "string",
                "index": "not_analyzed"
            },
            "name_ik": {
                "type": "string",
                "analyzer": "ik_max_word",
                "search_analyzer": "ik_max_word",
                "term_vector": "with_positions_offsets"
            },
            "name_only_pinyin": {
                "type": "string",
                "analyzer": "only_pinyin_analyzer",
                "search_analyzer": "only_pinyin_analyzer"
            },
            "name_full_pinyin": {
                "type": "string",
                "analyzer": "full_pinyin_analyzer",
                "search_analyzer": "full_pinyin_analyzer"
            },
            "title": {
                "type": "string",
                "index": "not_analyzed"
            },
            "cityId": {
                "type": "string",
                "index": "not_analyzed"
            },
            "fcn": {
                "type": "string",
                "index": "not_analyzed"
            },
            "scn": {
                "type": "string",
                "index": "not_analyzed"
            },
            "tags": {
                "type": "string",
                "index": "not_analyzed"
            },
            "onState": {
                "type": "integer"
            },
            "createdAt": {
                "type": "date"
            },
            "updatedAt": {
                "type": "date"
            }
        }
    }
}
```
***

# 1.1 /suggest

**简要描述：** 

- 获取商品搜索建议信息
	- 即搜索栏中输入中文时进行拼音**全拼**（“百事”为“baishi”）和**拼音首字母**（“百事”为“bs”）的识别和建议 
	- 输入拼音时，因无法分辨英文和拼音的区别，一律使用英文识别，即“百shi”即为“baishi”和“bshi”
    - 根据scroe（命中分数）排序
    - 已进行非（中文、英文、数字、下划线）的过滤
    - 仅搜索上架商品，同步频率为10s

**请求URL：** 
- ` /api/goods/shelf/suggest `
  
**请求方式：**
- POST

**参数：** 

|参数名|必选|类型|说明|
|:----|:---|:-----|-----|
|keyword |是  |string |商品预拼音，比如“百事”、“bai”、“baishi”，即用户在搜索栏输入的内容信息，无需过滤|
|cityId |是  |string |店铺所在城市编号，即xxxxxx六位数字，如320400|
|limit|否|int |偏移量/记录条数，默认为10，即需要返回的记录数量| 

**返回示例**

- 搜索“百事”时，返回如下：
``` 
{
    "status": 1,
    "message": "",
    "data": [
        "百事佳得乐 橙味 600ml*12瓶/箱",
        "百事佳得乐 柠檬味 600ml*12瓶/箱",
        "百事美年达 橙味 600ml*24瓶/箱",
        "百事可乐 2L*6瓶/箱",
        "百事佳得乐 蓝莓味 600ml*12瓶/箱",
        "百事美年达 蜜柚 600ml*12瓶/箱",
        "百事美年达 西瓜 600ml*12瓶/箱",
        "百事美年达 葡萄 600ml*12瓶/箱",
        "百事美年达 青苹果 600ml*12瓶/箱",
        "百事可乐330ml*24罐/箱"
    ]
}
```

 **返回参数说明** 

- data中Array即为建议的商品名称数组

***

# 1.2 /search

**简要描述：** 

- 获取用户输入关键词的搜索结果
    - 根据scroe（命中分数）排序
    - 已进行非（中文、英文、数字、下划线）的过滤
    - 仅搜索上架商品，同步频率为10s

**请求URL：** 
- ` /api/goods/shelf/search `
  
**请求方式：**
- POST 

**参数：** 

|参数名|必选|类型|说明|
|:----|:---|:-----|-----|
|sid   |是|int/string |用户编号|
|keyword |是  |string |关键词，已进行非（中文、英文、数字、下划线）的过滤|
|cityId |是  |string |店铺所在城市编号，即xxxxxx六位数字，如320400|
|offset|否|int |起始量，默认为0，和limit搭配使用，用于分页| 
|limit|否|int |偏移量/记录条数，默认为20，即需要返回的记录数量| 

 **返回示例**

- 搜索“可口 可乐”，limit为5时，返回如下：
``` 
 {
    "status": 1,
    "message": "",
    "data": [
        {
            "gid": "119988",
            "name": "太古<tags>可口</tags><tags>可乐</tags>2L*6瓶/箱",
            "score": 0.88502157
        },
        {
            "gid": "119989",
            "name": "太古<tags>可口</tags><tags>可乐</tags> 300ml*15瓶/箱",
            "score": 0.87810045
        },
        {
            "gid": "101480",
            "name": "太古<tags>可口</tags><tags>可乐</tags> 1.25L*12瓶/箱",
            "score": 0.87763345
        },
        {
            "gid": "383",
            "name": "太古<tags>可口</tags><tags>可乐</tags> 600ml*24瓶/箱",
            "score": 0.86257315
        },
        {
            "gid": "119991",
            "name": "太古<tags>可口</tags><tags>可乐</tags>易拉罐 330ml*24罐/箱",
            "score": 0.7594816
        }
    ],
    "count": 9
}
```

 **返回参数说明** 

|参数名||类型|说明|
|:----|:---|:---|:-----|
|count||int|总计符合条件的商品搜索结果条数|
|data|gid   |int     |商品编号|
||name  |string     |商品名称，其中`<tag>`为高亮命中词，用于在页面显示命中搜索关键词结果|
||score  |float     |命中分数，搜索关键词的命中分数，结果越匹配，分数越高，使用该分数进行排序|
