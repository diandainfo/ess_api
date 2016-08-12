# 更新日志

## 2016年8月9日16:21:30
* 升级ES，从v1.5.0到v2.3.4
* 使用elasticsearch-jdbc替换river，因v2.x之后river已从plugin移除
* 使用ik分词器([analysis-ik]())，完成更精确的分词匹配和评分体系
* 减少不必要的索引field，降低索引后docs大小
* 