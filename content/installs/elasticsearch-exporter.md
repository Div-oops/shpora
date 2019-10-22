---
title: "Elasticsearch Exporter"
date: 2019-10-22T11:39:25+03:00
draft: true
---

```
wget https://github.com/justwatchcom/elasticsearch_exporter/releases/download/v1.1.0/elasticsearch_exporter-1.1.0.linux-amd64.tar.gz
```
```
cat << EOF > /etc/systemd/system/elasticsearch_exporter.service
[Unit]
Description=Prometheus elasticsearch_exporter
After=local-fs.target network-online.target network.target
Wants=local-fs.target network-online.target network.target

[Service]
User=root
Nice=10
ExecStart = /usr/local/bin/elasticsearch_exporter -es.all -es.indices -es.timeout 20s
ExecStop= /usr/bin/killall elasticsearch_exporter

[Install]
WantedBy=default.target
EOF

systemctl daemon-reload
systemctl enable elasticsearch_exporter.service
systemctl start  elasticsearch_exporter.service
```
```
  - job_name: elasticsearch
    scrape_interval: 60s
    scrape_timeout:  30s
    metrics_path: "/metrics"
    static_configs:
    - targets:
      - elastic2.test.lan:9114
      - elastic-log2.prod.lan:9114
      labels:
        service: elasticsearch
```
