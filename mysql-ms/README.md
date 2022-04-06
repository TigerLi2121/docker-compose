启动服务
```shell
docker-compose up -d
```
查看服务ip地址
```shell
docker network inspect mysql-ms_msnet
```
进入master服务
```shell
docker exec -it mysql-master bash

mysql -uroot -proot@admin

# 查看server_id是否生效
show variables like '%server_id%';

# 看master信息  File 和 Position slave服务上要用
show master status;

# 新增用户并开权限
grant replication slave,replication client on *.* to 'slave'@'%' identified by "slave@admin";
flush privileges;
```
进入slave服务
```shell
docker exec -it mysql-slave bash

mysql -uroot -proot@admin

# 查看server_id是否生效
show variables like '%server_id%';

# 连接主mysql服务 master_log_file 和 master_log_pos的值要填写主master里查出来的值
# master_connect_retry 重试时间，单位秒，默认60
change master to master_host='172.31.0.3',master_user='slave',master_password='slave@admin',master_port=3306,master_log_file='mysql-bin.000003', master_log_pos=610,master_connect_retry=30;

# 启动slave
start slave;

# 查看状态
# 以下状态表示成功
# Slave_IO_Running: Yes         
# Slave_SQL_Running: Yes
show slave status \G;

# 设置slave服务为只读（可略）
SHOW VARIABLES LIKE '%read_only%'; #查看只读状态
SET GLOBAL super_read_only=1; #super权限的用户只读状态 1.只读 0：可写
SET GLOBAL read_only=1; #普通权限用户读状态 1.只读 0：可写
```

