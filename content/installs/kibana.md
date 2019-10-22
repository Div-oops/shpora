---
title: "Kibana"
date: 2019-10-22T11:40:29+03:00
draft: true
---

# [Home](https://div-oops.github.io/mugivar) [Installers](https://div-oops.github.io/mugivar/installers)
**1. Установка Kibana**

```
cd /tmp
wget https://artifacts.elastic.co/downloads/kibana/kibana-7.1.1-x86_64.rpm
rpm -ivh kibana-7.1.1-x86_64.rpm
```
**2. Настройка конфигурации Kibana**

```
#/etc/kibana/kibana.yml

server.port: 5601
server.host: "localhost"
elasticsearch.hosts: ["http://localhost:9200"]
```
**3. Старт службы**

```
sudo systemctl enable kibana
sudo systemctl start kibana
```
**4. Настройка nginx**

```
#/etc/nginx/conf.d/kibana.conf

server {
    listen 80;
 
    server_name elk-stack.co;
 
    auth_basic "Restricted Access";
    auth_basic_user_file /etc/nginx/.kibana-user;
 
    location / {
        proxy_pass http://localhost:5601;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```
**5. Настройка доступа**

```
sudo htpasswd -c /etc/nginx/.kibana-user admin
TYPE YOUR PASSWORD
```
**6. Перезагружаем nginx и проверяем доступ к kibana**

```
systemctl reload nginx
```
В браузере открываем url: http://elk-stack.co
**7. Установка и настройка Metricbeat**

```
curl -L -O https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-7.1.1-x86_64.rpm
sudo rpm -vi metricbeat-7.1.1-x86_64.rpm
metricbeat modules enable system //включаем system modul
metricbeat setup -e // Команда setupзагружает информационные панели Kibana. Если информационные панели уже настроены, пропустите эту команду
systemctl enable metricbeat
systemctl start metricbeat
```
Для добавления нового хоста достаточно установить на нем metricbeat
**Secure**
```
./bin/kibana-keystore create
./bin/kibana-keystore add elasticsearch.username
./bin/kibana-keystore add elasticsearch.password
```