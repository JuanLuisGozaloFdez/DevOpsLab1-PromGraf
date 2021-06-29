# DevOpsLab1 Prometheus Grafana: Node-exporter on EC2 instance

This document shows how to install a node-exporter on an EC2 instance.
Alternatively, you could decide to install the full docker-compose Lab on the EC2 instance directly.

## Installation process

This steps describe how to install Node-exporter on your EC2 instances, keeping in mind that apt-get has not a package for node-exporter alone (as it is included inside 'prometheus' package).

### 1. Connect to your EC2 instance

Execute this (or adduser --disabled-password --gecos "" prometheus)

```bash
$ useradd -m -s /bin/bash prometheus
$
```

Then, download node_exporter release from original repo

```bash
$ curl -L -O  https://github.com/prometheus/node_exporter/releases/download/v0.17.0/node_exporter-0.17.0.linux-amd64.tar.gz
$
$ tar -xzvf node_exporter-0.17.0.linux-amd64.tar.gz
$ mv node_exporter-0.17.0.linux-amd64 /home/prometheus/node_exporter
$ rm node_exporter-0.17.0.linux-amd64.tar.gz
$ chown -R prometheus:prometheus /home/prometheus/node_exporter
```

Finally, add the node_exporter as *systemd service*

```bash
$ tee -a /etc/systemd/system/node_exporter.service << END
[Unit]
Description=Node Exporter
Wants=network-online.target
After=network-online.target
[Service]
User=prometheus
ExecStart=/home/prometheus/node_exporter/node_exporter
[Install]
WantedBy=default.target
END
$
$ systemctl daemon-reload
$ systemctl start node_exporter
$ systemctl enable node_exporter
$
```

## Testing installation

To verify the installation, you can execute:

```bash
$ curl http://localhost:9100
$
```

that will show you a text message like this:

```html
<html>
   <head>
     <title>Node Exporter></title>
   </head>
   <body>
     <h1>Node Exporter</h1>
     <p><a href="/metrics">Metrics</a></p>
   </body>
</html>
```

and you would see also the metrics in

```bash
$ curl http://localhost:9100/metrics
$
```
