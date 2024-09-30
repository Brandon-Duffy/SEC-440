# Project 3 - Application Redundancy



## Configure U1, U2, U3

On all 3 boxes, ad new user/change hostname



```
sudo adduser brandon
sudo usermod -aG sudo brandon
sudo hostnamectl set-hostname 'hostname'
reboot
```

Configure Network Settings w/ Netplan:



```
sudo nano /etc/netplan/00-installer-config.yaml
```

Configure U1, U2, U3 the same way, changing the IP address to each host

<figure><img src=".gitbook/assets/image (8).png" alt=""><figcaption><p>U1, U2, U3 Config (Change IP - 201/202/203</p></figcaption></figure>



```
sudo netplan apply
```



## Install + Configure MariaDB

On the 3 VM's:



```
sudo apt update
sudo apt install mariadb-server

# configure MariaDB config file

sudo nano /etc/mysql/mariadb.conf.d/60-galera.cnf
```

<figure><img src=".gitbook/assets/image (9).png" alt=""><figcaption><p>U1, U2, U3 Galera Configuration</p></figcaption></figure>

For all three machines, all information will be exactly the same, minus:



```
wsrep_node_name
wsrep_node_address

# wsrep_cluster_name stays the same across all VM's.
```

Once the three configurations were complete, stop MariaDB services, then run:



```
sudo galera_new_cluster --- 3 times on U1, the Master server


sudo nc -zv 10.0.5.201 3306

# Run this across the three VMs, it should succeed if all is well.
```



## MariaDB Secure Installation

On U1:



```
sudo mariadb-secure-installation

# N to all questions


# Once complete, create a new user for MySQL for later PHP use:

sudo mysql -u root -p

MariaDB:

CREATE USER 'brandon-adm'@'%' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON *.* TO 'brandon-adm'@'%' WITH GRANT OPTIONS;
FLUSH PRIVILEGES;
```



## HAproxy Configuration Changes

HA1 + HA2 configuration files need to have a new frontend and backend:

<figure><img src=".gitbook/assets/image (10).png" alt=""><figcaption><p>HA1 / HA2 Configuration Changes</p></figcaption></figure>

Restart HAproxy after changes.







## Web Application Install / Configuration

On Web01 and Web02 I install php and mysql.



```
sudo yum install php php-mysqld
sudo setsebool -P httpd_can_network_connect_db on
sudo systemctl restart httpd

# Create PHP info file

sudo nano /etc/var/www/html/info.php

<?php
phpinfo():
?>

```

<figure><img src=".gitbook/assets/image (11).png" alt=""><figcaption><p>PHP Info Page</p></figcaption></figure>

