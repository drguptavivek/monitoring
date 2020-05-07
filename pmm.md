




sudo apt-get purge pmm-client
sudo apt-get install pmm2-client
sudo pmm-admin config --server-url=https://admin:aiims123@192.168.13.17:443 --server-insecure-tls


sudo pmm-admin config --server-insecure-tls --server-url=https://admin:aiims123@192.168.13.17:443

sudo nano  /etc/mysql/my.cnf 
sudo service mariadb status
sudo service mariadb  restart
sudo service mariadb status

sudo pmm-admin add mysql --username=pmm --password=pmm --query-source=perfschema ps-mysql 127.0.0.1:3306

sudo pmm-admin add mysql --username=pmm --password=pmm --query-source=perfschema ps-mysql 127.0.0.1:3306



All the following recommendation are to be followed for biomedical waste generated in Covid-19 patient care EXCEPT:

The frequency of cleaning clinical areas with suspected COVID cases are located should be ONE of the following: