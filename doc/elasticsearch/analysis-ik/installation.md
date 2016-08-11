
# Elasticsearch中ik分词插件的安装

**版本信息**
```
Elasticsearch:1.5.0
Ik:1.3.0
```

## 安装步骤：

* [进入plugins]
```
cd /usr/local/elasticsearch-1.5.0/plugins/
```
* [下载source] 
```
wget https://github.com/medcl/elasticsearch-analysis-ik/archive/v1.3.0.tar.gz
```
* [解压缩]
```
tar -zxvf v1.3.0.tar.gz
``` 
解压后生成`elasticsearch-analysis-ik-1.3.0`
* [进入文件路径] 
``` 
cd elasticsearch-analysis-ik-1.3.0
```
* [编译] 
```
mvn compile
```
* [打包] 
```
mvn package
```
* [安装] cd到Elasticsearch主路径
```
cd /usr/local/elasticsearch-1.5.0
```
使用
```
bin/plugin --install analysis-ik --url file:///usr/local/elasticsearch-1.5.0/plugins/elasticsearch-analysis-ik-1.3.0/target/releases/elasticsearch-analysis-ik-1.3.0.zip
```
* [复制config] 
```
cp /usr/local/elasticsearch-1.5.0/plugins/elasticsearch-analysis-ik-1.3.0//config/ik -rf /usr/local/elasticsearch-1.5.0/config
```
* [配置ik] 
```
vim /usr/local/elasticsearch-1.5.0/config/elasticsearch.yml
```
在最后追加
```
index:
  analysis:
    analyzer:
      ik:
          alias: [ik_analyzer]
          type: org.elasticsearch.index.analysis.IkAnalyzerProvider
      ik_max_word:
          type: ik
          use_smart: false
      ik_smart:
          type: ik
          use_smart: true
```
* restart elsticsearch ... gogogo
