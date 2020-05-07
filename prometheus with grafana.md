https://devconnected.com/how-to-setup-grafana-and-prometheus-on-linux/

# Configure Prometheus as a Grafana datasource and minotor a node
Installing the Node Exporter to monitor Linux metrics



```

  cd ~
wget https://github.com/prometheus/node_exporter/releases/download/v0.18.1/node_exporter-0.18.1.linux-amd64.tar.gz
tar -xvf node_exporter-0.18.1.linux-amd64.tar.gz
ls -al
 cd node_exporter-0.18.1.linux-amd64/
ls -al
sudo cp node_exporter /usr/local/bin
sudo useradd -rs /bin/false node_exporter
sudo chown node_exporter:node_exporter /usr/local/bin/node_exporter

```

Get the file [here](./node_exporter.service)  
```
cd /lib/systemd/system

sudo touch node_exporter.service
sudo nano node_exporter.service

```


```
sudo systemctl daemon-reload
sudo systemctl enable node_exporter
sudo systemctl start node_exporter
sudo systemctl status node_exporter

sudo journalctl -f -u node_exporter.service


sudo ufw allow 9100 comment "Node Exporter"
sudo ufw reload
sudo ufw show listening
```



# On Promethus server

```
sudo nano /etc/prometheus/prometheus.yml

   static_configs:
    - targets: ['localhost:9090', '192.168.13.50:9100']


```   

# On Grafana Server

Build a Grafana dashboard to monitor Linux metrics - Import the appropriate dashboard
