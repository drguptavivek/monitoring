



# Installation



```
sudo apt-get install -y apt-transport-https
sudo apt-get install -y software-properties-common wget
wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add -
sudo add-apt-repository "deb https://packages.grafana.com/oss/deb stable main"

sudo apt-get install grafana

```


# Immportant COnfig Files

```
sudo cat /usr/lib/systemd/system/grafana-server.service
# Server BINARY - 
ls -al /usr/sbin/grafana-server


# ENVIRONMENT VRAIBLES
sudo cat /etc/default/grafana-server 

GRAFANA_USER=grafana
GRAFANA_GROUP=grafana
GRAFANA_HOME=/usr/share/grafana
LOG_DIR=/var/log/grafana
DATA_DIR=/var/lib/grafana
MAX_OPEN_FILES=10000
CONF_DIR=/etc/grafana
CONF_FILE=/etc/grafana/grafana.ini
RESTART_ON_UPGRADE=true
PLUGINS_DIR=/var/lib/grafana/plugins
PROVISIONING_CFG_DIR=/etc/grafana/provisioning
# Only used on systemd systems
PID_FILE_DIR=/var/run/grafana

```

# Start Grafana


```
sudo systemctl start grafana-server
sudo systemctl status grafana-server

```

# Access Grafana
```
sudo ufw allow 3000 comment "Grafana"
sudo ufw reload
curl -v http://localhost:3000
http://192.168.13.50:3000
```
username- admin
password- admin
Change password

# Restrict Ananymous access and signup

```
sudo nano /etc/grafana/grafana.ini

# enable anonymous access
enabled = true shoiuld be false

;allow_sign_up = true --> change to false



sudo systemctl restart grafana-server
```

# Disable new user registrations