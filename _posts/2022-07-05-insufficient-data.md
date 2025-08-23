---
title: 'Zabbix and web monitoring'
date: 2025-01-15
permalink: /posts/2025/15/Zabbix for monitoring a website
excerpt_separator: <!--more-->
toc: true
tags:
  - career
  - cyber security
  - zabbix
---
## Zabbix for web monitoring portfolio 3 discussion
Zabbix is an open-source monitoring software specially crafted and built for monitoring various IT infrastructure components, such as web-sites, servers, network devices, applications, and services. It provides a holistic and comprehensive suite of features for real-time monitoring, alerting, and visualization of system health and performance. Zabbix is particularly popular in enterprise environments due to its scalability, flexibility, and open-source nature, making it highly customizable and cost-effective.

#### Key Features of Zabbix <!--more-->
- Real-time Monitoring: For network devices (switches), servers (physical and virtual servers, etc), applications (Web applications, databases, email systems, etc.), cloud environments (AWS, Azure, Google Cloud, etc.), storage devices (Disk usage, file systems, etc.).
- Data Collection: it consists of Agent-based monitoring (Zabbix agents are installed on monitored machines) and, Agentless monitoring (For devices or systems where agent installation is not possible or desirable, Zabbix can collect data via protocols like SNMP (Simple Network Management Protocol), IPMI (Intelligent Platform Management Interface), or SSH (Secure Shell).
- Visualization: It provides dashboards with customizable, real-time visual representations of the system’s health, graphs, and charts, as well as trend analysis of performance metrics such as CPU usage, memory usage, and network throughput.
- #### Other features
- Scalability, Auto-discovery, Security, Alerts and Notifications

### Zabbix portfolio network segmentation (create a VM sandbox)
This a continuation and an extension from my [previous post](/posts/2023/03/nest-map) on 'A Basic Sandboxing Portfolio', we are going to consider an extended network segmentation for this portfolio study.

#### Create an IP table for each machine on the network

| Device     | Role     | IP Address     | Subnet Mask     |
|--------------|--------------|--------------|--------------|
| Desktop 1 VM | Management and deployment | 192.168.200.9  | 255.255.255.0 |
| Desktop 2 as Wordpress server VM | Management and deployment | 192.168.200.5  | 255.255.255.0 |
| Gateway Router VM (enp0s3)  | Subnet 01 - Access to internet, Gateway and Desktop | 192.168.126.1  | 255.255.255.0 |
| Gateway Router VM (enp0s8)  | Subnet 02 - Access to internet and Gateway Desktop 1 and Desktop Wordpress server| 192.168.200.1  | 255.255.255.0 |
| Gateway Router VM (enp0s9)  |  Internet Access to Ubuntu Gateway  | 10.0.2.15  | 255.255.255.0 |
| Gateway Router VM (enp0s10)  | Subnet 03 - Access to internet, Gateway and Desktop | 192.168.26.1  | 255.255.255.0 |
| Application Server VM  | Server (Bitnami-Opencart)  | 192.168.26.2  | 255.255.255.0 |

- #### Desktop Configuration:
  - #### Desktop 1 for Management and deployment
Go to setting and then click NETWORK and set ADPATER 1 to INTERNAL NETWORK, then click OK and click START to bootup the machine on the VM.

After starting up the machine, go to Settings and click on WIRED SETTINGS then click on IDENTITY to setup the MAC Address to (08:00:27:97:75:31 (enp0s3)) then click IPV4 and set to Manual to set the ADDRESS, NETMASK, GATEWAY AND DNS ADDRESS: 192.168.200.9 NETMASK: 255.255.255.0 GATEWAY: 192.168.200.9	and DNS: 8.8.8.8, 8.8.4.4, 1.1.1.1 
Then click on APPLY and then DISCONNECT and RE-CONNECT the WIRED CONNECTION

Open a command line terminal using “Ctrl + alt + T” and type “ip a” to see if IP address is set, if IP address is set continue, else re-do the above step and restart the Ubuntu desktop machine. 

 - #### Desktop 2 for WORDPRESS mangement, installation and deployment
Go to setting and then click NETWORK and set ADPATER 1 to INTERNAL NETWORK, then clicked OK and click START to bootup the machine on the VM.

After starting up the machine, go to Settings and click on WIRED SETTINGS then click on IDENTITY to setup the MAC Address to (08:00:27:97:75:31 (enp0s3)) then click IPV4 and set to Manual to set the ADDRESS, NETMASK, GATEWAY AND DNS ADDRESS: 192.168.200.5	NETMASK: 255.255.255.0 GATEWAY: 192.168.200.1	and DNS: 8.8.8.8, 1.1.1.1 
Then click on APPLY and then DISCONNECT and RE-CONNECT the WIRED CONNECTION

Open a command line terminal using “Ctrl + alt + T” and type “ip a” to see if IP address is set, if IP address is set continue, else re-do the above step and restart the Ubuntu desktop machine. 

- #### **Command codes for installation of ZABBIX-AGENT and WORDPRESS**
<div style="background-color: #f0f8ff; border-left: 5px solid #007acc; padding: 10px; margin: 25px 0; font-style: italic; font-weight: bold;">
<p style="color: #333; font-size: 13px; line-height: 1.5;">
1.    sudo apt update<br>
2.    sudo apt install -y zabbix-agent<br>
Enter the zabbix configuration file<br>
3.    sudo nano /etc/zabbix/zabbix_agentd.conf<br>
Search manually for Server, ServerActive and Hostname (insert the host IP on which the server is stored for monitoring)<br>
4.    Server=192.168.200.2<br>
5.    ServerActive=192.168.200.2<br>
6.    Hostname=Zabbix server <!-- Zabbix server --> <span style="color: #007acc;">[Zabbix server]</span><br>
7.    sudo systemctl enable zabbix-agent <!-- Enable from system start-up --> <span style="color: #007acc;">[Enable from system start-up]</span><br>
8.    sudo systemctl start zabbix-agent <!-- To start agent installed --> <span style="color: #007acc;">[To start agent installed] </span><br>
9.    sudo systemctl status zabbix-agent <!-- To start agent installed --> <span style="color: #007acc;">[To check agent availability if active or not] </span><br>
    <a href="https://example-link.com" style="color: #007acc;"></a>
</p>
</div>

- #### Prequisite and dependecies for WORDPRESS installation 
To install WORDPRESS you need Apache2, Mysql, and Php <br>

- #### Steps to install Apache
<div style="background-color: #f0f8ff; border-left: 5px solid #007acc; padding: 10px; margin: 25px 0; font-style: italic; font-weight: bold;">
<p style="color: #333; font-size: 13px; line-height: 1.5;">
1.    sudo apt update<br>
2.    sudo apt install apache2 -y<!--To install apache server--> <span style="color: #007acc;">[apache server installation] </span><br>
3.    sudo systemctl restart apache2<!--to restart sever--><span style="color: #007acc;">[To restart server] </span><br>
4.    sudo systemctl status apache2<!--to check if active--><span style="color: #007acc;">[To check if active] </span><br>
5.    sudo systemctl enable apache2<!--to start from boot-up--><span style="color: #007acc;">[To start from boot-up] </span><br>
<a href="https://example-link.com" style="color: #007acc;"></a>
</p>
</div>
  
- #### Steps to install Mysql server for database management
  
<div style="background-color: #f0f8ff; border-left: 5px solid #007acc; padding: 10px; margin: 25px 0; font-style: italic; font-weight: bold;">
<p style="color: #333; font-size: 13px; line-height: 1.5;">
1.    sudo apt update<br>
2.    sudo apt install mysql-server<br>
3.    sudo systemctl restart mysql <!-- To restart the mysql --> <span style="color: #007acc;">[To restart the mysql]</span><br>
4.    sudo systemctl start mysql <!-- To start mysql installed --> <span style="color: #007acc;">[To start mysql installed] </span><br>
5.    sudo systemctl enable mysql <!-- To start apache from system-boot up --> <span style="color: #007acc;">[To start mysql from system-boot up] </span><br>
6.    sudo systemctl status mysql <!-- To check availability installed --> <span style="color: #007acc;">[To check mysql availability if active or not] </span><br>
7.    sudo mysql_secure_installation<!-- anwswer yes to all question in this section --> <span style="color: #007acc;">[anwswer yes to all question in this section] </span><br>
8.    sudo mysql -u root -p <!-- To enter mysql database interface --> <span style="color: #007acc;">[To enter mysql database interface ]</span><br>
9.    CREATE DATABASE wordpress; <!--In the mtsql enviroment enter this script to build wordpress database--> <span style="color: #007acc;">[in the mysql enviroment enter this script to build wordpress database] </span><br>
10.    CREATE USER 'wordpressuser'@'localhost' IDENTIFIED BY 'your_password';<!--To create user named 'wordpressuser' set your password --> <span style="color: #007acc;">[To create user named 'wordpressuser' and set your password] </span><br>
11.   GRANT ALL PRIVILEGES ON wordpress.* TO 'wordpressuser'@'localhost';<!--granting privileges and access--> <span style="color: #007acc;">[granting privileges and access] </span><br>
12.    FLUSH PRIVILEGES;<!--make all changes permanent--> <span style="color: #007acc;">[make all changes permanent] </span><br>
13.   EXIT;<!--To quit or leave--> <span style="color: #007acc;">[To quit or leave] </span><br> <a href="https://example-link.com" style="color: #007acc;"></a>
</p>
</div>

- #### Steps to install PHP server for database management

<div style="background-color: #f0f8ff; border-left: 5px solid #007acc; padding: 10px; margin: 25px 0; font-style: italic; font-weight: bold;">
<p style="color: #333; font-size: 13px; line-height: 1.5;">
1.    sudo apt update<br>
2.    sudo apt install php libapache2-mod-php php-mysql php-cli php-xml php-curl php-json php-mbstring php-zip php-gd php-intl<br><!-- php, libraries and other dependecies installation --> <span style="color: #007acc;">[php and other dependecies installation] </span><br>
3.    sudo apt install php-bcmath php-soap php-ldap php-imagick php-xsl php-opcache php-sqlite3 php-memcached php-redis<br><!-- compactibility dependecies installation -->
4.    sudo apt install php-fpm <!--(Optional fpm-php for nginx)--> <span style="color: #007acc;">[Optional fpm-php for nginx] </span><br>
5.    php -v <!-- To verify php and its version --> <span style="color: #007acc;">[To verify] </span><br>
6.   sudo a2enmod php <!-- Configure apache to work with php --> <span style="color: #007acc;">[Configure apache to work with php] </span><br>
7.   sudo systemctl restart apache2 <!-- Restarting apache --> <span style="color: #007acc;">[Restarting apache] </span><br>
<a href="https://example-link.com" style="color: #007acc;"></a>
</p>
</div>

- #### Steps to download install WORDPRESS

<div style="background-color: #f0f8ff; border-left: 5px solid #007acc; padding: 10px; margin: 25px 0; font-style: italic; font-weight: bold;">
<p style="color: #333; font-size: 13px; line-height: 1.5;">
1.    cd /var/www/html<br><!--Move into the html directory to download wordpress--> <span style="color: #007acc;">[Move into the html directory to download wordpress] </span><br>
2.    sudo wget https://wordpress.org/latest.tar.gz<br><!--To download wordpress installation scripts--> <span style="color: #007acc;">[To download wordpress installation scripts] </span><br>
3.    sudo tar -xvzf latest.tar.gz<br><!-- Extract the install zip file --><span style="color: #007acc;">[Extract the install zip file] </span><br>
4.    sudo rm latest.tar.gz<!--remove the archive zip file--> <span style="color: #007acc;">[remove the archive zip file] </span><br>
5.    cd wordpress <!-- Move to wordpress directory to configure --> <span style="color: #007acc;">[Move to wordpress directory to configure] </span><br>
6.   sudo cp wp-config-sample.php wp-config.php
<!-- Copy configuration file to create a new configuration file: --> <span style="color: #007acc;">[Copy configuration file to create a new configuration file] </span><br>
7.   sudo nano wp-config.php<!-- Edit the wp-config.php file--> <span style="color: #007acc;">[Edit the wp-config.php file] </span><br>
Find and modify the following lines in the wp-config.php file to match the database settings you created earlier<br>
8.   define( 'DB_NAME', 'wordpress' ); , define( 'DB_USER', 'wordpressuser' ); , define( 'DB_PASSWORD', 'your_password' ); , define( 'DB_HOST', 'localhost' ); <br>
9.   Save and exit the file (Ctrl + O, Enter, Ctrl + X).<br>
10.  sudo chown -R www-data:www-data /var/www/html/wordpress<!--Change ownership of WordPress files to the Apache user (www-data)--> <span style="color: #007acc;">[Change ownership of WordPress files to the Apache user (www-data)] </span><br>
11.   sudo find /var/www/html/wordpress -type d -exec chmod 755 {} \; <!--Set appropriate permissions for WordPress files and directories--> <span style="color: #007acc;">[Set appropriate permissions for WordPress files and directories] </span><br>
12.   sudo nano /etc/apache2/sites-available/wordpress.conf<!--Create a new Apache virtual host configuration file for WordPress--> <span style="color: #007acc;">[Create a new Apache virtual host configuration file for WordPress] </span><br>
13.   Add this configuration 
  <VirtualHost *:80><br>
  ServerAdmin webmaster@localhost<br>
  DocumentRoot /var/www/html/wordpress<br>
  ServerName your_domain_or_IP<br>
  <Directory /var/www/html/wordpress><br>
  AllowOverride All<br>
  Require all granted<br>
  </Directory><br>
  ErrorLog ${APACHE_LOG_DIR}/error.log<br>
  CustomLog ${APACHE_LOG_DIR}/access.log combined<br>
</VirtualHost><!--Add the following configuration (replace your_domain_or_IP with your domain or server IP)--> <span style="color: #007acc;">[Add the following configuration (replace your_domain_or_IP with your domain or server IP)] </span><br>

14.   sudo a2ensite wordpress.conf<br><!--Enble WORDPRESS the default site)--> <span style="color: #007acc;">[Enble WORDPRESS the default site] </span><br>
sudo a2dissite 000-default.conf<br><!--Disable the default site--> <span style="color: #007acc;">[disbale default site] </span><br>
15.   sudo a2enmod rewrite<br><!--Enable apache to write module--> <span style="color: #007acc;">[Enable apache to write module] </span><br>
16.   sudo systemctl restart apache2<!--Restart apache2--> <span style="color: #007acc;">[Restart apache2]</span><br>

Complete WORDPRESS installation online via Broswer (frontend)<br>
17.   http://your_domain_or_IP<!--Use you ip address on you brower. e.g http://192.168.200.5--> <span style="color: #007acc;">[Use you ip address on you brower. e.g http://192.168.200.5] </span><br>
 <a href="https://example-link.com" style="color: #007acc;"></a>
</p>
</div>

- #### Gateway(Router) Configuration:
  
  Go to settings and then click NETWORK and set ADAPTER 1 for (desktop 1) to INTERNAL NETWORK, ADAPTER 2 for (desktop 2) to INTERNAL NETWORK, ADAPTER 3 to NAT (Internet Access), and ADAPTER 4 to INTERNAL NETWORK for all 192.168.200.1 network.<br>
Then click OK and click START to bootup the machine on the VM <br>
After starting up the machine use:  <br>
Then use: student@router:~$ **ip a**				(to view all IPs) <br>
Set the network this way: view  [previous post](/posts/2023/03/nest-map) for indentation
<div style="background-color: #f0f8ff; border-left: 5px solid #007acc; padding: 10px; margin: 25px 0; font-style: italic; font-weight: bold;">
<p style="color: #333; font-size: 13px; line-height: 1.5;">
student@router:~$   sudo nano /etc/netplan/00-installer-config.yaml <br>
1.<br>
 network:<br>
       ethernets:<br>
        enp0s3:<br>
          dhcp4: no<br>
            addresses: [192.168.xxx.1/24]
        enp0s8:<br>
            dhcp4: no<br>
            addresses: [192.168.xxx.1/24]
        enp0s9:<br>
            dhcp4: true<br>
            addresses: []<br>
        enp0s8:<br>
            dhcp4: no<br>
            addresses: [192.168.xxx.1/24]<br>
version: 2 <br>
            
2.    sudo netplan apply <!--To apply changes--> <span style="color: #007acc;">[To apply changes]</span><br>
3.    student@router:~$ ip a<!--To view all IP setting--> <span style="color: #007acc;">[To view all IP setting]</span><br>
4.  sudo reboot<br>
    <a href="https://example-link.com" style="color: #007acc;"></a>
</p>
</div>
**Note** : For final configuration on IP table routing follow this [LINK](https://moodle.roehampton.ac.uk/pluginfile.php/4873277/mod_resource/content/4/Setting%20Up%20a%20Ubuntu%20Gateway%20Router_v02.pdf)<br>

- #### Bitnami Opencart Server Configuration:
 
