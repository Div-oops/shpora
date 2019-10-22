---
title: "Elasticsearch"
date: 2019-10-22T10:47:28+03:00
draft: true
---

**1. Создаем репозиторий**
```
#elasticsearch.repo 

[elasticsearch-7.x]
name=Elasticsearch repository for 7.x packages
baseurl=https://artifacts.elastic.co/packages/7.x/yum
gpgcheck=1
gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
enabled=1
autorefresh=1
type=rpm-md

```
**2. Установка openjdk и elasticsearch-7.1.1**
```
yum install java-1.8.0-openjdk
```
```
yum install elasticsearch-7.1.1-1

systemctl daemon-reload

systemctl enable elasticsearch.service
```
**3. Настройка системы **
```
# максимальное количество файловых хендлеров
echo 'elasticsearch  -  nofile  65536' >> /etc/security/limits.conf
# максимальное количество потоков (можно увеличить ещё, при необходимости)
echo 'elasticsearch  -  proc  2048' >> /etc/security/limits.conf

# отключение свопа
swapoff -a

#добавляем виртуальной памяти
echo 'vm.max_map_count=262144' > /etc/sysctl.d/99-elasticsearch.conf

sysctl -w vm.max_map_count=262144
```
**4. Редактирование конфигов**
```
#/etc/elasticsearch/jvm.options
[...]
-Xms2g
-Xmx2g
[...]
```

```
#/etc/elasticsearch/elasticsearch.yml

# ---------------------------------- Cluster -----------------------------------
cluster.name: my-claster //elasticsearh с одинаковыми именами кластеров в одной сети объединяются в один
# ------------------------------------ Node ------------------------------------
node.name: node-master-1 // должно быть уникальным в пределах одного кластера
node.master: true
node.data: false
# ----------------------------------- Paths ------------------------------------
path.data: /var/lib/elasticsearch
path.logs: /var/log/elasticsearch
# ----------------------------------- Memory ----------------------------------
bootstrap.memory_lock: true
# ---------------------------------- Network -----------------------------------
network.host: 0.0.0.0
http.port: 9200
network.publish_host: hostname
--------------------------------- Discovery ----------------------------------
discovery.seed_hosts: ["host1","host2"]
cluster.initial_master_nodes: ["node-master-1"]
```

```
#/etc/sysconfig/elasticsearch:

CONF_DIR=/etc/elasticsearch
MAX_LOCKED_MEMORY=unlimited
```

```
#/etc/security/limits.conf

elasticsearch  -  nofile  65536
elasticsearch  -  proc  2048
elasticsearch soft memlock unlimited
elasticsearch hard memlock unlimited
```

```
#/usr/lib/systemd/system/elasticsearch.service:

LimitMEMLOCK=infinity
```
**5. Скачиваем и устанавливаем плагин analysis-morphology для elasticsearch версии 7.1.1**
```
cd /tmp
wget https://github.com/oleg-st/elasticsearch-analysis-morphology/files/3314386/analysis-morphology-7.1.1.zip
cd /usr/share/elasticsearch/bin/
./elasticsearch-plugin install file:///tmp/analysis-morphology-7.1.1.zip
```
**6. Проверяем работоспособность**
```
systemctl daemon-reload
systemctl start elasticsearch.service

# проверяем, что оно работает — должен вернуть json с информацией о сервере
curl -XGET 'localhost:9200/?pretty'
#проверяем хост:
curl -X GET "localhost:9200/_cat/health?v&pretty"
#проверяем кластер
curl -X GET "localhost:9200/_cluster/health?pretty=true"
#проверяем индексы если они есть (после обновления индексов может не быть, особенно если менялся каталог с path.data:
curl -X GET 'localhost:9200/_cat/indices?v'
#информация о кластере и его узлах
curl -XGET 'http://localhost:9200/_cluster/state?pretty'
```
**Secure**
```
xpack.security.enabled: true

./bin/elasticsearch-setup-passwords interactive
```
