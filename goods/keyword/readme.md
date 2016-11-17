
# 商品品牌标签

## 1.1 page

**简要描述：** 

- 商品品牌标签 - 管理页面
	- 采用jsonp方式封装所有页面JavaScript逻辑及相关ajax请求
	- 只需使用以下代码即可集成到页面中
	  ```
	  <div id="goods_keyword_container"></div>
	
	  <!--jQeruy应在script前加载-->

	  <script src="/api/goods/keyword"></script>
	  ```

**请求URL：** 
- ` /api/goods/keyword `
  
**请求方式：**
- GET


## 1.2 ext

**简要描述：** 

- Elasticsearch-analysis-ik 调用远程扩展字典的链接
  - 具体说明见 [medcl/Elasticsearch-analysis-ik  >> 热更新 IK 分词使用方法](https://github.com/medcl/elasticsearch-analysis-ik#热更新-ik-分词使用方法) 
  - 其中 * Last-Modified 和 ETag * 的具体实现：
    - ETag
      - 说明见 [HTTP Header中的ETag](http://blog.csdn.net/chenzhiqin20/article/details/10947857) 
      - 可选实现node-module [jshttp/etag](https://github.com/jshttp/etag)
      - 使用[简单modified的timestamp]-[size]的方式实现etag
    - Last-Modified
      - [last modified file date in node.js](http://stackoverflow.com/questions/7559555/last-modified-file-date-in-node-js)
      - [node.js: browser image caching with correct headers](http://stackoverflow.com/questions/22201490/node-js-browser-image-caching-with-correct-headers)
    - 使用res.setHeader(key,value)方法写入对应header信息

**请求URL：** 
- ` /api/goods/keyword/ext `
  
**请求方式：**
- GET

