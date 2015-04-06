# note-install-elk

#Mô hình mutiple node
3 node: 
- node 1 : Logstash + JAVA
- node 2: Elastisearch + JAVA
- node 3: Kibana

Cài đặt như bình thường, chú ý khi cấu hình trỏ như sau
- node 1: Trỏ ip output elasticsearch trong filde cấu hình logstash đến node 2

<img src="http://i.imgur.com/vuOB7SW.png">

- node 3: Trỏ ip kibana về elasticsearch node 2 trong filde conf.js

<img src="http://i.imgur.com/UE67an2.png">

- Restast logstash ở node 1 và apahce2 ở node 2

------------

# Mô hình Ha với elasticsearch cluster

- node 1: Cài đặt JAVA - logstash - Kibana
- [Cluster]: gổm 2 node : node A - JAVA + ELASTICSEARCH, node B -JAVA + ELASTICSEARCH

Cài đặt như bình thường, khi cấu hình cấu hình như sau
- Kibana trỏ ip đến IP của một trong 2 node elasticsearh
- Logstash trỏ output `elasticsearch { cluster => "OurCluster" }`

<img src="http://i.imgur.com/FdBeW0x.png">

- node A cấu hình `vi /etc/elasticsearch/elasticsearch.yml` thêm 2 dòng sau

```sh
cluster.name: OurCluster
node.name: "esa"
```
<img src="http://i.imgur.com/MkJcp1a.png">

- node B cấu hình `vi /etc/elasticsearch/elasticsearch.yml` thêm 2 dòng sau
```sh
cluster.name: OurCluster
node.name: "esa"
```

<img src="http://i.imgur.com/lulKHLE.png">

Restart `elasticsearch` trên cả 2 node A và B

Kiểm tra lại bằng câu lệnh:

`curl http://localhost:9200/_nodes/process?pretty` nếu thấy có 3 node (node logstash, node A, node B là OK)

hoặc vào giao diện web `http://localhost:9200/_cluster/health?pretty=true` 

**Chú ý** localhost ở đây là ip của elasticsearch node, ví dụ node A, node B

<img src="http://i.imgur.com/BMTINIW.png">

thấy status = green là OK

-------------------------------------------

Tham khảo
- http://logstash.net/docs/1.4.0/outputs/elasticsearch
- http://webcache.googleusercontent.com/search?q=cache:HMgQHAk6wSoJ:techhari.blogspot.com/2013/03/elasticsearch-cluster-setup-in-2-minutes.html+&cd=5&hl=vi&ct=clnk
- http://www.genericarticles.com/mediawiki/index.php?title=Clustering_%26_Tuning_elastic_search#How_to_check_cluster_Health
- http://www.genericarticles.com/mediawiki/index.php?title=Clustering_%26_Tuning_elastic_search
