# MYSQL

### Create a user for the project
Having different user for each project is useful if you are going to run many apps on the same machine.

1. First Sign into mysql ` mysql -u root --password `
2. Inside the console
```
mysql> use mysql
mysql> CREATE USER 'project_name'@'localhost' IDENTIFIED BY 'projectpassword';
mysql> GRANT ALL ON *.* TO 'project_name'@'localhost';
mysql> FLUSH PRIVILEGES;
```

### Reset MYSQL root Password
in case you forgot...
```
sudo /etc/init.d/mysql stop
sudo service mysql stop
sudo mysqld_safe --skip-grant-tables &
mysql -uroot
```
Inside MYSQL console:
```
mysql> use mysql;
mysql> update user set password=PASSWORD("mynewpassword") where User='root';
mysql> flush privileges;
mysql> quit
```
Back to shell console
```
sudo /etc/init.d/mysql stop
sudo service mysql start
mysql -u root -p
```
