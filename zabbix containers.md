docker run --name zabbix-mysql-db -t \
-e MYSQL_DATABASE="zabbix" \
-e MYSQL_USER="zabbix" \
-e MYSQL_PASSWORD="zabbix123" \
-e MYSQL_ROOT_PASSWORD="zabbix123" \
-d mysql:5.7

docker run --name zabbix-server-mysql5 -t \
-e DB_SERVER_HOST="zabbix-mysql-db" \
-e MYSQL_DATABASE="zabbix" \
-e MYSQL_USER="zabbix" \
-e MYSQL_PASSWORD="zabbix123" \
-e MYSQL_ROOT_PASSWORD="zabbix123" \
--link zabbix-mysql-db:mysql \
-p 10051:10051 \
-d zabbix/zabbix-server-mysql:latest

docker run --name zabbix-web-apache-mysql8 -t \
-e DB_SERVER_HOST="zabbix-mysql-db" \
-e MYSQL_DATABASE="zabbix" \
-e MYSQL_USER="zabbix" \
-e MYSQL_PASSWORD="zabbix123" \
-e MYSQL_ROOT_PASSWORD="zabbix123" \
--link zabbix-mysql-db:mysql \
--link zabbix-server-mysql5:zabbix-server \
-p 9009:80 \
-d zabbix/zabbix-web-apache-mysql:latest
