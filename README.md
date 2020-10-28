# Kudu

## 简介
> kudu是专门为hadoop平台开发的列式存储管理器
- 列式存储
> 数据存储在强类型的列中，优势：1、高效读取指定列数据；2、针对单一数据类型的压缩效率高；
- Table
> 包含元数据信息和全局有序的主键，分为若干个tablets
- Tablet
> tablet可以复制多个副本，其中有一个主tablet;任意副本都可以提供读服务，而写需要tablet服务器达成共识。
- Tablet Server
> Server分为主Server和从Server，只有主Server可以提供写服务，而所有的Server都可以提供读服务；一个Server可以服务多个Tablet，一个Tablet可以被多个Server服务
- Master
> Master监控所有的Tablet,Tablet Server,Catalog Table以及集群相关的元数据信息；同一时间只能有一个Master，挂了之后新的Master可以通过选举产生；Master所有数据都存储在Tablet中，可以复制到到所有候选的Master；Tablet Server定期发送心跳信息给Master；
- Catalog Table
> catalog table是kudu元数据信息的集中地，保存table和tablet的的信息，不可以直接进行读写，只能通过客户端的api来访问

## 部署
### 安装
- 下载地址：[kudu6个rpm文件](https://archive.cloudera.com/cdh5/redhat/7/x86_64/cdh/5.15.1/RPMS/x86_64/)
- 安装命令：`sudo rpm -ivh --nodeps *`
### 配置（/etc/kudu/conf）
- master.gflagfile(配置master元数据目录)
```sh
# Do not modify these two lines. If you wish to change these variables,
# modify them in /etc/default/kudu-master.
--fromenv=rpc_bind_addresses
--fromenv=log_dir

--fs_wal_dir=/data/kudu/kudu_master_data
--fs_data_dirs=/data/kudu/kudu_master_data
```
- tserver.gflaggfile(table数据目录)
>
```sh
# Do not modify these two lines. If you wish to change these variables,
# modify them in /etc/default/kudu-tserver.
--fromenv=rpc_bind_addresses
--fromenv=log_dir

--fs_wal_dir=/data/kudu/kudu_tserver_data
--fs_data_dirs=/data/kudu/kudu_tserver_data
--tserver_master_addres=hadoop000:7051
```
### 启动（/etc/init.d/）
- master 启动  
`sudo ./kudu-master start`
- server 启动  
`sudo ./kudu-tserver start`
### 环境
- 部署启动报错及解决方法  
1. `./kudu-master: line 47: /lib/lsb/init-functions: No such file or directory`
> `sudo yum install redhat-lsb -y`
2. `Cannot initialize clock: Error reading clock. Clock considered unsynchronized`
> `sudo yum install ntp`  
> `service ntpd start`  
[操作链接](https://blog.csdn.net/qq_36329973/article/details/104477318)
### ui界面
> `http://hadoop000:8050/`