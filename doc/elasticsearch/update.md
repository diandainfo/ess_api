# 更新日志

## 2016年8月9日16:21:30
* 升级ES，从v1.5.0到v2.3.4
* 使用elasticsearch-jdbc替换river，因v2.x之后river已从plugin移除
* 使用ik分词器([analysis-ik]())，完成更精确的分词匹配和评分体系
* 减少不必要的索引field，降低索引后docs大小
* 新增ES-plugin:deleteByQuery，及对应的定时任务，用于定时清理已下架的商品
* 新建log_error和log_info用于存储系统错误日志和信息日志

## 2016年8月12日14:24:22
* 替换pinyin的使用方法，使用match替换completion suggester，已解决update/delete index后suggester查询的doc不更新的bug
* 将log_error和log_info的set转移到request中，直接使用ES的port解决高并发的承载问题

## 2016年8月18日15:45:29
* 重现pinyin+ik的搜索建议、搜索关键词方案，完成开发任务

## 2016年8月25日10:46:21
* 开始编写pinyin+ik，搜索实现方案文档