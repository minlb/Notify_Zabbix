# Cấu hình Zabbix-proxy
## 1. IP các server
IP của các Server:
Zabbix-server: 192.168.0.104
Zabbix-proxy: 192.168.0.105
Zabbix-agent1: 192.168.0.103 với hostname trên cấu hình web: Centos 7
Zabbix-agent2: 192.168.0.106 với hostname trên cấu hình web: Centos 6

## 2. Cấu hình Zabbix-proxy:
- Thêm repo để cài Zabbix-proxy và MariaDB nếu dùng centos 7, ubuntu 16
- Cài đặt Zabbix-proxy và MariaDB
- Cấu hình Database trong mariadb và Zabbix-proxy
- Cấu hình trên Zabbix-agent và Zabbix-Server WEB

## 3. Thực hiện
### 3.1 Thêm repo
- Zabbix
  Centos: 
  
      rpm -ivh http://repo.zabbix.com/zabbix/3.2/rhel/7/x86_64/zabbix-release-3.2-1.el7.noarch.rpm
      
  Ubuntu: 
  
       # wget http://repo.zabbix.com/zabbix/3.2/ubuntu/pool/main/z/zabbix-release/zabbix-release_3.2-1+xenial_all.deb
       # dpkg -i zabbix-release_3.2-1+xenial_all.deb
       # apt-get update
- MariaDB
  Centos: vi /etc/yum.repo.d/Mariadb.repo
          
          [mariadb]
          name = MariaDB
          baseurl = http://yum.mariadb.org/10.2/centos7-amd64
          gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
          gpgcheck=1
  Ubuntu:
  
        $ sudo apt install software-properties-common
        $ sudo apt-key adv --recv-keys --keyserver hkp://keyserver.ubuntu.com:80 0xF1656F24C74CD1D8
        $ sudo vim /etc/apt/sources.list.d/mariadb.list
        $ deb [arch=amd64,i386] http://mirror.jmu.edu/pub/mariadb/repo/10.2/ubuntu xenial main
        $ deb-src http://mirror.jmu.edu/pub/mariadb/repo/10.2/ubuntu xenial main
        $ sudo apt update
        $ sudo apt install mariadb-server

### 3.2 Cài đặt
- Zabbix:
        yum install zabbix-proxy-mysql or apt install zabbix-proxy-mysql
- Mariadb: 
        yum install mariadb or apt install mysql
### 3.3 Cấu hình:
Mariadb:

        service mysql start
        Thực hiện: mysql_secure_installtion;
        mysql -u root -p;
        create database zabbix;
        create user 'zabbix'@'localhost' identified by 'minh';
        grant all privileges on * . * to 'zabbix'@'localhost';
        flush privileges;
Zabbix_proxy:
Import database zabbix: zcat /usr/share/doc/zabbix-proxy-mysql-3.2.X/schema.sql.gz | mysql -uzabbix -p   zabbix
Cấu hình:  vim /etc/zabbix/zabbix_proxy.conf
         
         Server=IP-Zabbix-Server(192.168.0.4)
         Hostname=zbx_proxy
         DBName=zabbix
         DBUser=zabbix
         DBPassword=minh 
Khởi động service: service zabbix-proxy start && systemctl enable zabbix-proxy

Zabbix-server-web:
![](image/Zabbix1.png)

### 3.4: Cấu hình trên Zabbix-server-web và Agent

- Zabbix-server-web:

![](image/zabbix2.png)
![](image/zabbix3.png)

- Zabbix-agent:

          Chỉnh file: vim /usr/local/etc/zabbix_agentd.conf
          Server=Proxy_Servers_IP (192.168.0.5)
          ServerActive=Proxy_Servers_IP (192.168.0.5)
          Hostname=Hostname in web ( Centos 7 or Centos 6)
      
Restart service: service zabbix-agent restart

