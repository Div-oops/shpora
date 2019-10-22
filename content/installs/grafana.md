---
title: "Grafana"
date: 2019-10-22T11:40:17+03:00
draft: true
---

# [Home](https://div-oops.github.io/mugivar) [Installers](https://div-oops.github.io/mugivar/installers)
```
vim /etc/yum.repos.d/grafana.repo

[grafana]
name=grafana
baseurl=https://packages.grafana.com/oss/rpm
repo_gpgcheck=1
enabled=1
gpgcheck=1
gpgkey=https://packages.grafana.com/gpg.key
sslverify=1
sslcacert=/etc/pki/tls/certs/ca-bundle.crt
```
```
sudo yum install grafana
```
```
sudo systemctl start grafana-server.service 
sudo systemctl enable grafana-server.service 

```
## Dashboard
```
1860 Node Exporter Full
6742 PostgreSQL Statistics
355 Postgres Overview
2322 Elasticsearch
2347 Elasticsearch Stats
3254 Elasticsearch All
3002 memory
```
### Plagins
```
grafana-cli plugins install grafana-piechart-panel
```
