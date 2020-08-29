---
title: Oracle CDB PDB 表空间创建用户及角色
date: 2020-08-25 21:39:31
comment: true
categories: ORACLE
tags:
- ORACLE
- TABLESPACE
- SCHEMA
- ROLE
---
记录如何使用11g以上版本oracle
<!--more-->

**在进行如下操作之前，需保证Docker在running，然后启动ORACLE**

# 登录ORACLE
``` bash
docker container ls
```
``` bash
docker exec -it 3024386a1d24 /bin/bash
```
```bash
dockerfiles [remotes/origin/revert-1451-ssl_enable~53] % docker container ls
CONTAINER ID        IMAGE                         COMMAND                  CREATED             STATUS                        PORTS                                            NAMES
3024386a1d24        oracle/database:12.2.0.1-ee   "/bin/sh -c 'exec $O…"   43 minutes ago      Up About a minute (healthy)   0.0.0.0:1521->1521/tcp, 0.0.0.0:5500->5500/tcp   oracle
dockerfiles [remotes/origin/revert-1451-ssl_enable~53] % docker exec -it 3024386a1d24 /bin/bash
[oracle@3024386a1d24 ~]$ sqlplus / as sysdba

SQL*Plus: Release 12.2.0.1.0 Production on Sat Aug 29 12:32:19 2020

Copyright (c) 1982, 2016, Oracle.  All rights reserved.

ERROR:
ORA-12162: TNS:net service name is incorrectly specified


Enter user-name: ^C^C^C
[oracle@3024386a1d24 ~]$ 
[oracle@3024386a1d24 ~]$ 
[oracle@3024386a1d24 ~]$ 
[oracle@3024386a1d24 ~]$ ps -ef | grep ora_
oracle      23     1  0 12:30 ?        00:00:00 ora_pmon_ORCLCDB
oracle      25     1  0 12:30 ?        00:00:00 ora_clmn_ORCLCDB
oracle      27     1  0 12:30 ?        00:00:00 ora_psp0_ORCLCDB
oracle      29     1  0 12:30 ?        00:00:00 ora_vktm_ORCLCDB
oracle      33     1  0 12:30 ?        00:00:00 ora_gen0_ORCLCDB
oracle      35     1  0 12:30 ?        00:00:00 ora_mman_ORCLCDB
oracle      39     1  0 12:30 ?        00:00:00 ora_gen1_ORCLCDB
oracle      43     1  0 12:30 ?        00:00:00 ora_diag_ORCLCDB
oracle      45     1  0 12:30 ?        00:00:00 ora_ofsd_ORCLCDB
oracle      49     1  0 12:30 ?        00:00:00 ora_dbrm_ORCLCDB
oracle      51     1  0 12:30 ?        00:00:00 ora_vkrm_ORCLCDB
oracle      53     1  0 12:30 ?        00:00:00 ora_svcb_ORCLCDB
oracle      55     1  0 12:30 ?        00:00:00 ora_pman_ORCLCDB
oracle      57     1  0 12:30 ?        00:00:00 ora_dia0_ORCLCDB
oracle      59     1  0 12:30 ?        00:00:00 ora_dbw0_ORCLCDB
oracle      61     1  0 12:30 ?        00:00:00 ora_lgwr_ORCLCDB
oracle      63     1  0 12:30 ?        00:00:00 ora_ckpt_ORCLCDB
oracle      65     1  0 12:30 ?        00:00:00 ora_lg00_ORCLCDB
oracle      67     1  0 12:30 ?        00:00:00 ora_smon_ORCLCDB
oracle      69     1  0 12:30 ?        00:00:00 ora_lg01_ORCLCDB
oracle      71     1  0 12:30 ?        00:00:00 ora_smco_ORCLCDB
oracle      73     1  0 12:30 ?        00:00:00 ora_reco_ORCLCDB
oracle      75     1  0 12:30 ?        00:00:00 ora_w000_ORCLCDB
oracle      77     1  0 12:30 ?        00:00:00 ora_lreg_ORCLCDB
oracle      79     1  0 12:30 ?        00:00:00 ora_w001_ORCLCDB
oracle      81     1  0 12:30 ?        00:00:00 ora_pxmn_ORCLCDB
oracle      85     1  0 12:30 ?        00:00:01 ora_mmon_ORCLCDB
oracle      87     1  0 12:30 ?        00:00:00 ora_mmnl_ORCLCDB
oracle      89     1  0 12:30 ?        00:00:00 ora_d000_ORCLCDB
oracle      91     1  0 12:30 ?        00:00:00 ora_s000_ORCLCDB
oracle      93     1  0 12:30 ?        00:00:00 ora_tmon_ORCLCDB
oracle     103     1  0 12:30 ?        00:00:00 ora_tt00_ORCLCDB
oracle     105     1  0 12:30 ?        00:00:00 ora_tt01_ORCLCDB
oracle     107     1  0 12:30 ?        00:00:00 ora_tt02_ORCLCDB
oracle     109     1  0 12:30 ?        00:00:00 ora_aqpc_ORCLCDB
oracle     113     1  0 12:30 ?        00:00:00 ora_p000_ORCLCDB
oracle     115     1  0 12:30 ?        00:00:00 ora_p001_ORCLCDB
oracle     117     1  0 12:30 ?        00:00:00 ora_p002_ORCLCDB
oracle     119     1  0 12:30 ?        00:00:00 ora_p003_ORCLCDB
oracle     121     1  0 12:30 ?        00:00:00 ora_p004_ORCLCDB
oracle     123     1  0 12:30 ?        00:00:00 ora_p005_ORCLCDB
oracle     125     1  0 12:30 ?        00:00:00 ora_p006_ORCLCDB
oracle     127     1  0 12:30 ?        00:00:00 ora_p007_ORCLCDB
oracle     129     1  0 12:30 ?        00:00:00 ora_p008_ORCLCDB
oracle     131     1  0 12:30 ?        00:00:00 ora_p009_ORCLCDB
oracle     133     1  0 12:30 ?        00:00:00 ora_p00a_ORCLCDB
oracle     135     1  0 12:30 ?        00:00:00 ora_p00b_ORCLCDB
oracle     137     1  0 12:30 ?        00:00:00 ora_p00c_ORCLCDB
oracle     139     1  0 12:30 ?        00:00:00 ora_p00d_ORCLCDB
oracle     141     1  0 12:30 ?        00:00:00 ora_p00e_ORCLCDB
oracle     143     1  0 12:30 ?        00:00:00 ora_p00f_ORCLCDB
oracle     286     1  1 12:30 ?        00:00:01 ora_cjq0_ORCLCDB
oracle     368     1  0 12:30 ?        00:00:00 ora_qm02_ORCLCDB
oracle     372     1  0 12:30 ?        00:00:00 ora_q002_ORCLCDB
oracle     374     1  0 12:30 ?        00:00:00 ora_q003_ORCLCDB
oracle     376     1  0 12:30 ?        00:00:00 ora_q004_ORCLCDB
oracle     411     1  0 12:31 ?        00:00:00 ora_w002_ORCLCDB
oracle     464   412  0 12:32 pts/0    00:00:00 grep --color=auto ora_
[oracle@3024386a1d24 ~]$ export ORACLE_SID=ORCLCDB
[oracle@3024386a1d24 ~]$ sqlplus / as sysdba
```

# 修改Sys/System的密码
修改密码是便于自己使用跟设置SqlDeveloper，使用JDBC
```sql
ALTER USER sys IDENTIFIED BY YOURPASSWORD
```

# 预备知识
需要知道CDB跟PDB是怎么回事，Oracle 12C引入了CDB与PDB的新特性，在ORACLE 12C数据库引入的多租用户环境（Multitenant Environment）中，允许一个数据库容器（CDB）承载多个可插拔数据库（PDB）。CDB全称为Container Database，中文翻译为数据库容器，PDB全称为Pluggable Database，即可插拔数据库。在ORACLE 12C之前，实例与数据库是一对一或多对一关系（RAC）：即一个实例只能与一个数据库相关联，数据库可以被多个实例所加载。而实例与数据库不可能是一对多的关系。当进入ORACLE 12C后，实例与数据库可以是一对多的关系。下面是官方文档关于CDB与PDB的关系图
 ![ORACLE](oracle.jpg)

其实大家如果对SQL SERVER比较熟悉的话，这种CDB与PDB是不是感觉和SQL SERVER的单实例多数据库架构是一回事呢。像PDB$SEED可以看成是master、msdb等系统数据库，PDBS可以看成用户创建的数据库。而可插拔的概念与SQL SERVER中的用户数据库的分离、附加其实就是那么一回事。看来ORACLE也“抄袭”了一把SQL SERVER的概念，只是改头换面的包装了一番。

## CDB组件（Components of a CDB）
一个CDB数据库容器包含了下面一些组件：
**ROOT组件：**
    ROOT又叫CDB$ROOT, 存储着ORACLE提供的元数据和Common User,元数据的一个例子是ORACLE提供的PL/SQL包的源代码，Common User 是指在每个容器中都存在的用户。

**SEED组件：**
    Seed又叫PDB$SEED,这个是你创建PDBS数据库的模板，你不能在Seed中添加或修改一个对象。一个CDB中有且只能有一个Seed. 这个感念，个人感觉非常类似SQL SERVER中的model数据库。

**PDBS：**
    CDB中可以有一个或多个PDBS，PDBS向后兼容，可以像以前在数据库中那样操作PDBS，这里指大多数常规操作。
这些组件中的每一个都可以被称为一个容器。因此，ROOT(根)是一个容器，Seed(种子)是一个容器，每个PDB是一个容器。每个容器在CDB中都有一个独一无二的的ID和名称。

### 查看当前容器
```sql
sql> SHOW CON_NAME;

CON_NAME 
------------------------------
CDB$ROOT
```
### 查看CDB中可用的PDB
```sql
sql> SELECT
    CON_ID,DBID,NAME,OPEN_MODE
FROM
    V$PDBS;
```
 ![PDBS](pdbs.png)

## 切换至PDB
因为sql开发是需要在PDB上进行，所以需要切换至PDB中，只写两种方式，其中**方式1**是笔者常用
方式1:
```sql
ALTER SESSION SET CONTAINER = ORCLPDB1;
```
方式2:
```sql
alter pluggable database ORCLPDB1 open;;
```

# 使用SqlDeveloper 方便写sql开发
## 配置SqlDeveloper
为了区分，建议按DB类型进行分开, cdb的用户名跟密码是可以登录pdb的，下面都可以使用前面设置好的sys的用户名，当然你也可以只登录到CDB，然后使用上面的切换方式
 ![SQLDEVELOPER](sqldeveloperoverview.png)
### 使用Basic类型配置CDB
**注意正确选择Role**

![CDB Config](cdbconfig.png)
注：图中的**Service name/SID（当前是一样的）** 就是你在上面命令行中export的**ORACLE_SID**
### 使用Basic类型配置PDB
![PDB Config](pdbconfig.png)
### 使用JDBC配置PDB/CDB
**只需要把上图中的Connection Type改成Advanced**
JDBC URL:
```bash
jdbc:oracle:thin:@localhost:1521/orclpdb1
```
## 使用Sql Developer进行开发
### 创建TableSpace

