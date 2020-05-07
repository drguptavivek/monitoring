
based on steps in
MAIN
https://devconnected.com/how-to-setup-grafana-and-prometheus-on-linux/

GRAFANA - Please see the file  [here](./grafana.md) 

https://devconnected.com/how-to-install-grafana-on-ubuntu-18-04/


Supplementary
https://pepipost.com/tutorials/setup-prometheus-and-exporters/


https://www.centlinux.com/2019/11/install-prometheus-monitoring-server-centos-7.html


# Download and extarct

visit - https://github.com/prometheus/prometheus/releases/
Note latest version
MOdify WGET link below
```
apt-cache policy prometheus

wget https://github.com/prometheus/prometheus/releases/download/v2.18.0/prometheus-2.18.0.linux-amd64.tar.gz

tar xzf prometheus-2.18.0.linux-amd64.tar.gz
mv prometheus-2.18.0.linux-amd64 prometheus
cd prometheus
ll
```
* *prometheus*: It's a binary file which is the core daemon.
* *prometheus.yml*: This is the config file for Prometheus service.
* *promtool*: This is another binary file which is used to compile the alert rules file. 


# Install in proper locations and give permissions
```
sudo useradd -M -r -s /bin/false prometheus

sudo cp prometheus promtool /usr/local/bin
sudo chown prometheus:prometheus /usr/local/bin/prometheus
# sudo chown prometheus:prometheus /usr/local/bin/prometool
cd /usr/local/bin
ls -al

sudo mkdir /etc/prometheus 
cd /etc/prometheus 
sudo cp -R consoles/ console_libraries/ prometheus.yml /etc/prometheus
sudo chown -R prometheus:prometheus /etc/prometheus/*
ls -al

sudo mkdir -p /data/prometheus
cd /data/prometheus
sudo chown -R prometheus:prometheus /data/prometheus 
ls -al

```

# Create Service
```
cd /lib/systemd/system
sudo touch prometheus.service
sudo nano prometheus.service


[Unit]
Description=Prometheus
Wants=network-online.target
After=network-online.target

[Service]
Type=simple
User=prometheus
Group=prometheus
ExecStart=/usr/local/bin/prometheus \
--config.file=/etc/prometheus/prometheus.yml \
--storage.tsdb.path="/data/prometheus" \
--web.console.templates=/etc/prometheus/consoles \
--web.console.libraries=/etc/prometheus/console_libraries \
--web.listen-address=0.0.0.0:9090 \
--web.enable-admin-api

Restart=always

[Install]
WantedBy=multi-user.target




sudo systemctl disable --now cockpit.socket 
sudo systemctl stop  cockpit.socket 

sudo systemctl enable prometheus
sudo systemctl start prometheus
sudo systemctl status prometheus

http://192.168.13.48:9090/graph

```


# Proxying behind Nginx


```
sudo apt-get install nginx

sudo nano /etc/nginx/sites-available/prometheus.conf

server {
    listen 1234;
    location / {
      proxy_pass           http://localhost:9090/;
    }
}


sudo ln -s /etc/nginx/sites-available/prometheus.conf /etc/nginx/sites-enabled/
ls /etc/nginx/sites-enabled/
sudo systemctl restart nginx
sudo systemctl status nginx
sudo journalctl -f -u nginx.service


sudo ufw allow 1234/tcp comment "Prometheus"
sudo ufw reload

http://192.168.13.48:1234/graph



cd /lib/systemd/system 
sudo nano prometheus.service

ExecStart=/usr/local/bin/prometheus \
  --config.file=/etc/prometheus/prometheus.yml \
  --storage.tsdb.path="/data/prometheus" \
  --web.console.templates=/etc/prometheus/consoles \
  --web.console.libraries=/etc/prometheus/console_libraries \
  --web.listen-address=0.0.0.0:9090 \
  --web.enable-admin-api \
  --web.external-url=https://0.0.0.0:1234


sudo systemctl daemon-reload
sudo systemctl restart prometheus
journalctl -f -u prometheus.service


```

# Enable reverse proxy authentication

```
sudo apt-get install apache2-utils

sudo apt-get install gnutls-bin
cd /etc/ssl
sudo mkdir prometheus
cd prometheus
sudo certtool --generate-privkey --outfile prometheus-private-key.pem
sudo certtool --generate-self-signed --load-privkey prometheus-private-key.pem --outfile prometheus-cert.pem

cd /etc/prometheus
sudo htpasswd -c .credentials admin 




sudo nano /etc/nginx/sites-available/prometheus.conf


server {
    listen 1234 ssl http2;
    
    ssl_certificate /etc/ssl/prometheus/prometheus-cert.pem;
    ssl_certificate_key /etc/ssl/prometheus/prometheus-private-key.pem;

    location / {
        auth_basic           "Prometheus";
        auth_basic_user_file /etc/prometheus/.credentials;
        proxy_pass           http://localhost:9090/;
    }
}


sudo systemctl restart nginx
journalctl -f -u nginx.service


curl -u admin -k https://localhost:1234/metrics


```