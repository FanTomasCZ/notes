### Templates

* https://superuser.com/questions/1016290/how-to-edit-a-zabbix-trigger-for-a-specific-host
* https://www.zabbix.com/documentation/4.0/manual/config/macros/usermacros
* https://www.zabbix.com/documentation/4.0/manual/api/reference/template/update
* https://www.zabbix.com/documentation/4.0/manual/config/items/item
* https://github.com/v-zhuravlev/zbx-smartctl
* https://habr.com/ru/company/zabbix/blog/337856/
* https://bitbucket.org/sivann/runcached/src/master/

RegEx
* https://www.zabbix.com/documentation/3.4/manual/regular_expressions
* `^(?:\S+\s){2}(\S+)`: `RouterOS 6.44.3 (stable) on RB4011iGS+` ==> `6.44.3`

```
Name: Zabbix MySQL DB Size
Key: mysql.size[zabbixdb]
Units: b
Update interval: 1d

Make sure /var/lib/zabbix/.my.cnf file exists, has attr 600 and contains
[mysql]
user=zabbixuser
password=Password
host=localhost
```


### Agent Installation

```shell
# Ubuntu 18.04
mkdir -p ~/temp && cd ~/temp
wget https://repo.zabbix.com/zabbix/4.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_4.0-2+bionic_all.deb
dpkg -i zabbix-release_4.0-2+bionic_all.deb
apt update
apt install zabbix-agent zabbix-sender

# Configure agent
cp /etc/zabbix/zabbix_agentd.conf{,.bak}
nano /etc/zabbix/zabbix_agentd.conf
# Edit the following lines
# Server=192.168.10.2
# For active checks:
# ServerActive=192.168.10.2
# [!] Comment out Hostname parameter (and use HostnameItem instead)
# Hostname=server1.example.com
HostnameItem=system.hostname

# Restart agent
systemctl restart zabbix-agent
```
Add host to Zabbix Server
* Login to Zabbix server interface, and go to `Configuration` > `Hosts` > `Create host`
* Set Hostname
* Set IP address or DNS name
* Got to the `Templates` tab and select templates you want to use (:warning: `Add` then `Update`)

Debugging
```shell
# On server
zabbix_get -s ip-of-your-agent -k agent.ping
zabbix_get -s ip-of-your-agent -k agent.version
zabbix_get -s ip-of-your-agent -k agent.hostname
```

### Server Installation

* https://www.zabbix.com/life_cycle_and_release_policy
* https://www.zabbix.com/documentation/4.0/manual/installation/install_from_packages/debian_ubuntu

```shell
# Ubuntu 18.04
apt install apache2 libapache2-mod-php \
            php php-pear php-cgi php-common libapache2-mod-php php-mbstring php-net-socket php-gd \
            php-mysql php-gettext php-bcmath \
            mariadb-server
            
mysql_secure_installation

wget https://repo.zabbix.com/zabbix/4.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_4.0-2+bionic_all.deb
dpkg -i zabbix-release_4.0-2+bionic_all.deb
apt update

apt install zabbix-server-mysql zabbix-frontend-php zabbix-agent zabbix-get

cp /etc/zabbix/apache.conf{,.bak}
nano /etc/zabbix/apache.conf
# Uncomment and edit php_value date.timezone in <IfModule mod_php7.c>
# php_value date.timezone Europe/Kaliningrad

cp /etc/php/7.2/apache2/php.ini{,.bak}
nano /etc/php/7.2/apache2/php.ini
# Uncomment and edit date.timezone
# date.timezone = Europe/Kaliningrad

systemctl reload apache2

mysql -u root -p

create database zabbixdb character set utf8 collate utf8_bin;
grant all privileges on zabbixdb.* to zabbixuser@localhost identified by 'Password';
flush privileges;
exit;

zcat /usr/share/doc/zabbix-server-mysql/create.sql.gz | mysql -u zabbixuser -p zabbixdb

cp /etc/zabbix/zabbix_server.conf{,.bak}
nano /etc/zabbix/zabbix_server.conf
# Uncomment and edit the following parameters
DBHost=localhost
DBName=zabbixdb
DBUser=zabbixuser
DBPassword=Password

systemctl enable zabbix-server && systemctl start zabbix-server
systemctl enable zabbix-agent && systemctl start zabbix-agent
```
Navigate to `http://ip_address/zabbix` or `http://host_name/zabbix` and finish configuration <br>
Leave default values except for the following
* Database name: zabbixdb
* User: zabbixuser
* Password: Password
* Name: My Zabbix Server

 The default user name is `Admin` and the password is `zabbix`