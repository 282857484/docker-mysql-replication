# Docker image for MySQL master-slave replication

## Additional environment variables:
* REPLICATION_USER [default: replication]
* REPLICATION_PASSWORD [default: replication_pass]
* REPLICATION_HEALTH_GRACE_PERIOD [default: 3]
* REPLICATION_HEALTH_TIMEOUT [default: 10]
* MASTER_PORT [default: 3306]
* MASTER_HOST [default: master]

## Start master

```
docker run -d \
 --name mysql_master \
 -v /Users/yons/data/master:/var/lib/mysql \
 --net=bridge \
 -p 40001:3306 \
 -e MYSQL_ROOT_PASSWORD=mysqlroot \
 -e MYSQL_USER=example_user \
 -e MYSQL_PASSWORD=mysqlpwd \
 -e MYSQL_DATABASE=example \
 -e REPLICATION_USER=replication_user \
 -e REPLICATION_PASSWORD=myreplpassword \
 actency/docker-mysql-replication:5.7

```

## Start slave

```
docker run -d \
 --name mysql_slave \
 -v /Users/yons/data/slave:/var/lib/mysql \
 --net=bridge \
 -p 40002:3306 \
 -e MASTER_PORT=40001 \
 -e MASTER_HOST=10.1.1.1 \
 -e MYSQL_ROOT_PASSWORD=mysqlroot \
 -e MYSQL_USER=example_user \
 -e MYSQL_PASSWORD=mysqlpwd \
 -e MYSQL_DATABASE=example \
 -e REPLICATION_USER=replication_user \
 -e REPLICATION_PASSWORD=myreplpassword \
 --link mysql_master:master \
 actency/docker-mysql-replication:5.7
```

## Check replication status

```
docker exec -it mysql_slave mysql -uroot -pmysqlroot -e "SHOW SLAVE STATUS\G;"
```
