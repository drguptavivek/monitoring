# SSH CONGIG on Client

Please see the ssh_configs  [here](./ssh_configs)  
```
cp ~/Dropbox/VIVEK/Computer\ Facility\ AIIMS/CONFIGS/ssh_configs ~/.ssh/config     

cp /f/Dropbox/VIVEK/Computer\ Facility\ AIIMS/CONFIGS/ssh_configs ~/.ssh/config 

```

## ON SERVER - SSH, TimeZone, Hostname, UFW etc Disable SSH password
```
ssh-keygen -b 4096

sudo sed -i 's/#PasswordAuthentication.*/PasswordAuthentication no/' /etc/ssh/sshd_config
sudo service ssh restart
```



## ON SERVER - HOSTNAME and NTP
```
sudo hostnamectl  set-hostname HOSTNAME
sudo sed -i "s/preserve_hostname:.*/preserve_hostname: true/" /etc/cloud/cloud.cfg
hostnamectl

sudo timedatectl set-timezone Asia/Kolkata
sudo sed -i "s/#NTP=.*/NTP=192.168.185.233/"  /etc/systemd/timesyncd.conf 
sudo sed -i "s/#FallbackNTP=ntp.ubuntu.com*/FallbackNTP=ntp.ubuntu.com/" /etc/systemd/timesyncd.conf 

sudo systemctl restart systemd-timesyncd.service 
sudo systemctl status systemd-timesyncd.service 
timedatectl


sudo apt install qemu-guest-agent

## Ubuntu 18.04
sudo nano /etc/cloud/cloud.cfg.d/subiquity-disable-cloudinit-networking.cfg
            network: {config: disabled}

sudo sed -i 's/192.168.13.199\/24.*/192.168.13.49\/24/'  /etc/netplan/00-installer-config.yaml

## GO TO CONSOLE OF SERVER
sudo netplan try
sudo netplan apply




```

## UFW

```
sudo ufw allow 22 comment 'SSH'
sudo ufw allow 80 comment 'WEB'
sudo ufw allow 443 comment 'WEB SSL'
sudo ufw allow 53 comment 'DNS'
sudo ufw allow 123/udp comment 'NTP TimeSync'

sudo ufw status
sudo ufw enable

sudo ufw status numbered
sudo ufw show added
```
## Utilities

```
sudo apt install tree tmux htop vim curl libcurl4 wget  apt-transport-https  ca-certificates  nmap whois  inetutils-traceroute net-tools cpu-checker 
```


## Cockpit

```
sudo apt install cockpit cockpit-packagekit cockpit-networkmanager cockpit-system cockpit-storaged

sudo systemctl start cockpit
sudo systemctl enable --now cockpit.socket
sudo systemctl status cockpit


sudo systemctl disable --now cockpit.socket


# Restrict Cockpit Access to Trusted  IP only
sudo ufw delete allow 9090
sudo ufw allow proto tcp from  192.168.13.0/24 to any port 9090 comment 'Cockpit'
sudo ufw reload
```

## UFW Optional Series
```
# OPTIONAL SERIES
sudo ufw allow 1194/udp comment 'OpenVPN'
sudo ufw allow 3306 comment 'MariaDB'
sudo ufw allow 5432 comment 'PostGres'

sudo ufw allow 143 comment 'IMAP'
sudo ufw allow 993 comment 'IMAP SSL'
sudo ufw allow 995 comment 'POP3 SSL'
sudo ufw allow 110 comment 'POP3 unEncrypt'

sudo ufw allow 25 comment 'SMTPD unEncrypt'
sudo ufw allow 587 comment 'SMTPD TLS'
sudo ufw allow 465 comment 'SMTPD SSL'

sudo ufw delete allow 25

sudo ufw reload

sudo ufw show added
sudo ufw status numbered
sudo ufw show listening

```

## Zabbix Agent Install and Config 
Please see the file  [here](./Zabbix_agent_config.md) 
