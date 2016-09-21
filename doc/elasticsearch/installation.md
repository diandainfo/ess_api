
# Installation

**安装Elasticsearch_v2.3.4环境**

## Step 

### 1. Elasticsearch
> [https://www.elastic.co/downloads/elasticsearch](https://www.elastic.co/downloads/elasticsearch)

  * 下载2.3.4版本ES
  ```
    wget https://download.elastic.co/elasticsearch/release/org/elasticsearch/distribution/zip/elasticsearch/2.3.4/elasticsearch-2.3.4.zip
    
  ```
  * 解压缩，生成`elasticsearch-2.3.4`的目录
 
  ```
    unzip elasticsearch-2.3.4.zip
  ```
  * 进入生成的目录，修改配置，**`#为注释`**
    * 进入目录 
    ```
      cd elasticsearch-2.3.4
      vim config/elasticsearch.yml
    ```
    * 修改配置，`key: value`的":"后边加一个" "(空格)
    ![es配置节点参数](./images/es-config-1.png)
    ![es配置网略参数](./images/es-config-2.png)
    ```
      cluster.name: diandainfo                       # es运行时进程名、集群名，用于判断是否处于同一集群
      
      node.name: exp-1                               # 节点名称，每个节点不同，可根据[环境+序号]构成
      
      network.host: 192.168.1.100                    # 访问地址，0.0.0.0是内外网均可访问
      
      http.port: 9200                                # 访问端口
      
      transport.tcp.port: 9300                       # 节点间通信端口，默认为9300，用于判断是否处于同一集群且可以通过此端口进行同步
      
      transport.tcp.compress: true                   # 节点通信是否使用压缩，默认为不使用
    ```
    
    * 使用[Modules » Scripting](https://www.elastic.co/guide/en/elasticsearch/reference/current/modules-scripting.html#modules-scripting)时，需要追加配置参数
    	* updateByQuery 时用到script脚本，需要配置[script权限参数](https://www.elastic.co/guide/en/elasticsearch/reference/current/modules-scripting.html#enable-dynamic-scripting)
      	```
           script.inline: true
           script.indexed: true
           script.file: true
      	```
      	* 需要安装[JavaScript Language Plugin](https://www.elastic.co/guide/en/elasticsearch/plugins/2.4/lang-javascript.html)
      	```
	   bin/plugin install lang-javascript
      	```
  * 启动ES
  
  ```
	bin/elasticsearch
  ``` 
  * 测试
  
  ```
    curl -XGET http://localhost:9200
  ```

### 2. plugin-head
> [https://github.com/mobz/elasticsearch-head#installing-and-running](https://github.com/mobz/elasticsearch-head#installing-and-running)

  * 安装
  
  ```
	elasticsearch/bin/plugin install mobz/elasticsearch-head
  ```
  * (无需)重启ES
  * 浏览器访问 `[es地址]/_plugin/head`

### 3. Elasticsearch-analysis-ik
> [https://github.com/medcl/elasticsearch-analysis-ik#install](https://github.com/medcl/elasticsearch-analysis-ik#install)

  * 在`elasticsearch/plugins`[下载zip](https://github.com/medcl/elasticsearch-analysis-ik/releases)
  
  ```
    wget https://github.com/medcl/elasticsearch-analysis-ik/releases/download/v1.9.4/elasticsearch-analysis-ik-1.9.4.zip

  ```
  * 解压后放到`elasticsearch/plugins/analysis-ik`中
  * 重启ES，无报错即可

### 4. Elasticsearch-analysis-pinyin
> [https://github.com/medcl/elasticsearch-analysis-pinyin/releases](https://github.com/medcl/elasticsearch-analysis-pinyin/releases)
 
  * 下载zip

  ```
	wget https://github.com/medcl/elasticsearch-analysis-pinyin/releases/download/v1.7.4/elasticsearch-analysis-pinyin-1.7.4.zip

  ```
  * 解压后放到`elasticsearch/plugins/analysis-pinyin`
  * 重启ES，无报错即可

### 5. Elasticsearch-deletebyquery
> [https://github.com/dermidgen/elasticsearch-deletebyquery#setup](https://github.com/dermidgen/elasticsearch-deletebyquery#setup)

  * 直接安装
  
  ```
	bin/plugin install delete-by-query
  ```

### 6. Elasticsearch-jdbc

* es-jdbc在2.3.4.0版本中需要jre的版本在8以上

> [https://github.com/jprante/elasticsearch-jdbc#installation](https://github.com/jprante/elasticsearch-jdbc#installation)

  * 下载zip，最好是在es外文件夹中进行操作
  
  ```
	wget http://xbib.org/repository/org/xbib/elasticsearch/importer/elasticsearch-jdbc/2.3.4.0/elasticsearch-jdbc-2.3.4.0-dist.zip
  ```
  * 解压缩，生成`elasticsearch-jdbc-2.3.4.0`文件夹
  * 进入`elasticsearch-jdbc-2.3.4.0/bin`文件夹，修改配置文件
  * 按照river的结构修改`.sh`脚本，注意参数的变化
	* [https://github.com/jprante/elasticsearch-jdbc#parameters](https://github.com/jprante/elasticsearch-jdbc#parameters) 
