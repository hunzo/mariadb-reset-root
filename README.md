# reset script mariadb version 10.5+
## start db and mount volume with init script
### docker-compose.yml
```
    ...
    volumes: 
      - ./data:/var/lib/mysql
      - ./mysql-init/init.sql:/tmp/init.sql
    ...
```
### init.sql
```
USE mysql;
-- UPDATE user SET authentication_string=PASSWORD('YOURPASSWORD') WHERE User='root'; for MySQL 5.7.6+
-- UPDATE user SET PASSWORD=PASSWORD('YOURPASSWORD') WHERE User='root'; 
SET PASSWORD FOR 'root' = PASSWORD("YOURPASSWORD"); 
FLUSH PRIVILEGES;
```
### start container 
```shell
docker-compose run db bash
```
### exec mysqld_safe
```shell
mysqld_safe --init-file=/tmp/init.sql &
```
### test login root with new password and exit
```shell
mysql -u root -p
```
### stop container and remove docker
```
docker-compose stop db; docker-compose rm db;
```