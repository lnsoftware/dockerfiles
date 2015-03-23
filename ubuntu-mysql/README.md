# MySQL on Ubuntu 14.04 Docker container

 - based on https://github.com/docker-library/mysql/tree/master/5.7


## How to start a `mysql` server instance

	docker run -d --name mysqldb -e MYSQL_ROOT_PASSWORD=mysql -e MYSQL_DATABASE=dbname -e MYSQL_USER=dbuser -e MYSQL_PASSWORD=1dbpasswd! -p 23:22 -p 3306:3306 ubuntu/mysql57