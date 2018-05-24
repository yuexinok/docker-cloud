## 说明：

用于在本机docker环境下快速搭建分布式集成环境，包含mariadb(mysql),redis,ELK,Zookeeper等



## 1）mysql运行和安装：

运行实例：mysql_3306,mysql_3307,mysql_3308

对应本机端口：3306，3307，3308

对应配置文件：mysql/my.cnf

```ini
[client]
default-character-set = utf8mb4

[mysqld]
character_set_server = utf8mb4
collation_server = utf8mb4_bin
sql_mode = NO_ENGINE_SUBSTITUTION,NO_AUTO_CREATE_USER
```

```shell
#进入mysql目录：docker-compose up -d
➜  mysql git:(master) ✗ docker-compose up -d
Creating network "mysql_docker-cloud" with driver "bridge"
Creating mysql_mysql-3307_1 ... done
Creating mysql_mysql-3306_1 ... done
Creating mysql_mysql-3308_1 ... done

#查看
➜  mysql git:(master) ✗ docker-compose ps
       Name                    Command             State           Ports
---------------------------------------------------------------------------------
mysql_mysql_3306_1   docker-entrypoint.sh mysqld   Up      0.0.0.0:3306->3306/tcp
mysql_mysql_3307_1   docker-entrypoint.sh mysqld   Up      0.0.0.0:3307->3306/tcp
mysql_mysql_3308_1   docker-entrypoint.sh mysqld   Up      0.0.0.0:3308->3306/tcp
```

连接密码：-u root -p 123456

## 2) redis主从模式：

运行实例：redis_master redis_slave

开放端口：6379(主)，7000（从）

对应从配置文件：

```ini
slaveof redis_master 6379
```

```shell
➜  redis git:(master) ✗ redis-cli
127.0.0.1:6379> info
# Server
redis_version:4.0.9
.........

# Replication
role:master
connected_slaves:1
slave0:ip=172.20.0.3,port=6379,state=online,offset=70,lag=1
................


127.0.0.1:6379> set key1 1234
OK
127.0.0.1:6379> get key1
"1234"
127.0.0.1:6379> exit
➜  redis git:(master) ✗ redis-cli -p 7000
127.0.0.1:7000> get key1
"1234"
```



## 参考链接：

Docker-compose.xml文件编写：https://www.jianshu.com/p/2217cfed29d7

Docker-compose命令详解：https://blog.csdn.net/wanghailong041/article/details/52162293

官方文档：https://docs.docker.com/compose/compose-file