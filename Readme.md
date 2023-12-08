# VPS Setup - CentOS 8


## Get root access
---
- Check the current user
```
whoami
```

- Get root access for current user
```
sudo su
```

&nbsp;

## Set SElinux to permissive
---
- Set SElinux to permissive (To set permanently, change value in "/etc/selinux/config")
```
sudo setenforce 0
```

- Check status of SElinux
```
sudo getenforce
```

&nbsp;

## Change the repo URL to point to official CentOS vault repos
---
```
sed -i 's/mirrorlist/#mirrorlist/g' /etc/yum.repos.d/CentOS-*
sed -i 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-*
```

&nbsp;

## Install git
---
```
sudo dnf install git -y
```

&nbsp;

## Install node.js
---
- List out the available streams for the node.js module
```
sudo dnf module list nodejs
```

- Install Node.js 16
```
sudo dnf module install nodejs:16/common -y
```

- Alternative: Install Node.js 18
```
dnf install -y gcc-c++ make 
curl -sL https://rpm.nodesource.com/setup_18.x | sudo -E bash -
sudo dnf install nodejs -y
```

&nbsp;

## Install pm2
---
- Install process manager (pm2)
```
sudo npm install -g pm2
```

- Start application with pm2
```
pm2 start app.js --name AppName --wait-ready --listen-timeout 4000 --watch --ignore-watch="FolderName"
```

- Saving the application list to be restored at reboot
```
pm2 save
```

- Manually restore saved application list at reboot
```
pm2 resurrect
```

&nbsp;

## Install nginx
---
- List out the available streams for the nginx module
```
sudo dnf module list nginx
```

- Install the selected nginx stream
```
sudo dnf module install nginx:1.20/common -y (example: nginx 1.20)
```

- Start nginx service
```
sudo systemctl start nginx
```

- Add nginx service to system startup of CentOS 8
```
sudo systemctl enable nginx
```

- Install nano text editor
```
sudo dnf install nano
```

- Edit nginx config
```
sudo nano /etc/nginx/nginx.conf
```

- Check syntax of nginx config
```
sudo nginx -t
```

- Restart nginx service
```
sudo systemctl restart nginx
```

- Create SSL Certificate.crt from .pem file for nginx
```
cat your_domain_name_cert.pem CA_bundle.crt >> certificate.crt
```

- Tuning kernel parameter. Place [this file](https://github.com/AnthonyGunardi/Linux-kernel-config) into "/etc/sysctl.d/nginx-tuning.conf"
```
sudo sysctl -p /etc/sysctl.d/nginx-tuning.conf
```

&nbsp;

## Install postgreSQL
---
- List out the available streams for the postgresql module
```
sudo dnf module list postgresql
```

- Install the selected postgresql stream (example: postgresql 13)
```
sudo dnf module install postgresql:13/server -y
```

- Initialize postgresql database directory
```
sudo postgresql-setup --initdb
```

- Start postgresql service
```
sudo systemctl start postgresql
```

- Add postgresql service to system startup of CentOS 8
```
sudo systemctl enable postgresql
```

- Create new postgresql database (example: testdb)
```
sudo -u postgres createdb testdb
```

&nbsp;

## Install MariaDB
---
- List out the available streams for the postgresql module
```
sudo dnf module list mariadb
```

- Install the selected mariadb stream
```
sudo dnf module install mariadb:10.5/server -y (example: mariadb 10.5)
```

- Start mariadb service
```
sudo systemctl start mariadb
```

- Add mariadb service to system startup of CentOS 8
```
sudo systemctl enable mariadb
```

- Set secure installation
```
sudo mysql_secure_installation
```

- Login to MariaDB shell (example: username: root)
```
mysql -u root -p
```

- Create custom root account
```
CREATE USER 'your username'@'%' IDENTIFIED BY 'your password';
```

- Allow Remote Access to MariaDB
```
GRANT ALL PRIVILEGES ON *.* TO 'your username'@'%' IDENTIFIED BY 'your password' WITH GRANT OPTION;
```

- Create new mariadb database (example: testdb)
```
CREATE DATABASE testdb
```
