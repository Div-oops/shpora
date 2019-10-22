---
title: "Filebeat"
date: 2019-10-22T11:40:07+03:00
draft: true
---

```
yum install filebeat
```
```
vim /etc/filebeat/filebeat.yml

#=========================== Filebeat inputs =============================
filebeat.inputs:
- type: log
  enabled: true
  paths:
    - /var/log/nginx/doc-access.log
#================================ Outputs =====================================
#-------------------------- Elasticsearch output ------------------------------
#output.elasticsearch:
#  hosts: ["elastic_server:9200"]

#----------------------------- Logstash output --------------------------------
output.logstash:
  hosts: ["logstash_server:5044"]

