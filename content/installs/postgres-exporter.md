---
title: "Postgres Exporter"
date: 2019-10-22T11:41:06+03:00
draft: true
---

```
cd /tmp
curl -LO https://github.com/wrouesnel/postgres_exporter/releases/download/v0.5.1/postgres_exporter_v0.5.1_linux-amd64.tar.gz
tar -xvf postgres_exporter_v0.5.1_linux-amd64.tar.gz 
sudo mv postgres_exporter_v0.5.1_linux-amd64/postgres_exporter /usr/local/bin/
```
```
sudo useradd -rs /bin/false postgres_exporter
sudo vi /etc/systemd/system/postgres_exporter.service


[Unit]
Description=Postgres Exporter
After=network.target

[Service]
User=postgres_exporter
Group=postgres_exporter
Type=simple
ExecStart=/usr/local/bin/postgres_exporter

[Install]
WantedBy=multi-user.target


```
```
sudo systemctl daemon-reload
sudo systemctl start postgres_exporter
sudo systemctl enable postgres_exporter
```
