# Create MYSQL User
1. First Sign into mysql ` mysql -u root --password `
2. Inside the console
```
mysql> use mysql
mysql> CREATE USER 'project_name'@'localhost' IDENTIFIED BY 'projectpassword';
mysql> GRANT ALL ON *.* TO 'project_name'@'localhost';
mysql> FLUSH PRIVILEGES;
```
