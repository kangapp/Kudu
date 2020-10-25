# Kudu

## 简介
> kudu是专门为hadoop平台开发的列式存储管理器

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