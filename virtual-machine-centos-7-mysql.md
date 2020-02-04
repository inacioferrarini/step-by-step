# About

This guide provides a step-by-step approach in order to create a Virtual Machine running CentOS 7 and MySQL 8.

# Requirements

* Virtual Box 6.1

* CentOS-7-x86_64-Minimal-1908.iso disk image.

# Getting Started

## Virtual Machine Setup

Create a Virtual Machine (Linux, Red Hat (64-bit))
* **RAM** 1024 MB.
* **DISK**: 8 GB.

**Settings** > **Network** > **Adapter 1**: Change its type to **Bridged Adapter**.

# Network Setup

To rename the server
```bash
hostnamectl set-hostname #new name#
```

To install MySQL
```bash
rpm -Uvh https://repo.mysql.com/mysql80-community-release-el7-3.noarch.rpm

sed -i 's/enabled=1/enabled=0/' /etc/yum.repos.d/mysql-community.repo

yum --enablerepo=mysql80-community install mysql-community-server

service mysqld start

grep "A temporary password" /var/log/mysqld.log

mysql_secure_installation

service mysqld restart

chkconfig mysqld on
```

Create a firewall rule to allow 3306 access.
```bash
sudo firewall-cmd --zone=public --add-port=3306/tcp --permanent
```

In order to allow MySQL root access from any host
```bash
update user set host='%' where host='localhost';
```