
# Installation
> https://www.elastic.co/downloads/kibana

## Doc
* [Kibana4 - 《ELK stack中文指南》](http://kibana.logstash.es/content/kibana/v4/)

## Installation step
* Download and unzip Kibana 4
```
wget https://download.elastic.co/kibana/kibana/kibana-4.5.4-linux-x64.tar.gz

tar -zxvf kibana-4.5.4-linux-x64.tar.gz
```
* Open config/kibana.yml in an editor 
```
cd kibana-4.5.4-linux-x64
vim config/kibana.yml
```
* Set the elasticsearch.url to point at your Elasticsearch instance
```
# The Elasticsearch instance to use for all your queries.
elasticsearch.url: "http://localhost:9210"
```
* Run ./bin/kibana
  * [Remove ability to set kibana environment via NODE_ENV](https://github.com/elastic/kibana/issues/6388) 
  * just like
  ```
  NODE_ENV=development bin/kibana
  ```
