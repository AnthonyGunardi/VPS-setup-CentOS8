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

- Install the selected node.js stream (example: nodejs 16)
```
sudo dnf module install nodejs:16/common -y
```

- Update node.js to latest stable version
```
npm cache clean -f
npm install -g n
sudo -E env "PATH=$PATH" n stable
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
sudo dnf install mariadb-server
```

- Sart mariadb service
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

- Login to MariaDB shell
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