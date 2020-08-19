---
title: 如何在MacOS上使用Docker安装Oracle
date: 2020-08-19 21:10:24
comment: true
categories: MacOS
tags:
- MacOS
- Docker
- Oracle
---
本篇文章旨在备忘当初是如何在自己的Mac上安装并使用Oracle的。
<!--more-->
# 1 前期准备工作

由于自己的电脑是Mac，并且在工作中也是一直使用Oracle，但是难受的地方是，商业化做的不错的oracle竟然没有MacOS的安装包。木得办法，兵来将挡，水来土堰，办法总比困难多，想办法解决呀！

## Step 1.1 clone/下载Oracle的Docker镜像
主要是需要oracke的Dockerfile， get docker images here:
 https://github.com/oracle/docker-images
 下载完成后，进入你刚刚下载完成的路径中，如下代码：
 ```bash
 cd docker-images/OracleDatabase/SingleInstance/dockerfiles
```

 ## Step 2 下载Oracle的数据库的instance
链接URL： http://www.oracle.com/technetwork/database/enterprise-edition/downloads/index.html
download an oracle database and place it in the equvilent version folder, such as 12.2.0.1, then run below command.
下载的时候需要有Oracle账户，相信即便没有，申请也很容易。
下载完成后，把下载的zip包放到**对应版本号**的文件中，比如你下载的是12.2.0.1版本的，然后使用如下命令：
```bash
./buildDockerImage.sh -v 12.2.0.1 -e
```
此步骤比较耗时，大概有**21**个步骤要跑。要耐心等待，如果出现链接超时的情况，基本就是第**7**步的时候。强烈建议你早晨早点起床，肯定就能秒过第**7**步，不要问为什么，实践出真知。哈哈哈哈

 ## Step 2.1 安装oracle instance并映射Docker中的端口及数据文件到本地端口及本地路径 
 map to local port and data file
- 注意修改命令行中的路径(第一个冒号之前的路径)
```bash
docker run --name oracle -p 1521:1521 -p 5500:5500 -v /Users/${user}/idocker/oradata:/opt/oracle/oradata oracle/database:12.2.0.1-ee
```
注意数据文件已经map到 （/你上面指定路径/oradata）下。此时就算你删除了container，这些数据文件还是会被保留的。
对应的log如下，注意到了最后，会停在那里，估计run命令最后调用类似docker logs -f oracle这样的命令，会tail -f类似的输出，所以我们可以直接**新建一个窗口**使用了
```bash
docker stop oracle
```
再
```bash
docker start oracle
```
如果不想敲命令行启动oracle，你也可以使用Docker的Dashboard来点点鼠标就可以了。
![Docker Dashboard](snapshot/dockerdashboard.jpg)

**经过如上步骤之后，如果没错误，Oracle就是成功安装了。**
**因为是使用的Oracle 12 以上版本，所以你需要了解Oracle的Container Database（CDB） 跟 Plugin Database（PDB）的区别。**

参考log如下：
```bash
$ docker run --name oracle -p 1521:1521 -p 5500:5500 -v /Users/example/oradata:/opt/oracle/oradata oracle/database:12.2.0.1-ee
ORACLE PASSWORD FOR SYS, SYSTEM AND PDBADMIN: scXX7Cj+1m0=1
 
LSNRCTL for Linux: Version 12.2.0.1.0 - Production on 20-MAY-2017 14:25:30
 
Copyright (c) 1991, 2016, Oracle.  All rights reserved.
 
Starting /opt/oracle/product/12.2.0.1/dbhome_1/bin/tnslsnr: please wait...
 
TNSLSNR for Linux: Version 12.2.0.1.0 - Production
System parameter file is /opt/oracle/product/12.2.0.1/dbhome_1/network/admin/listener.ora
Log messages written to /opt/oracle/diag/tnslsnr/c9f09116cc83/listener/alert/log.xml
Listening on: (DESCRIPTION=(ADDRESS=(PROTOCOL=ipc)(KEY=EXTPROC1)))
Listening on: (DESCRIPTION=(ADDRESS=(PROTOCOL=tcp)(HOST=0.0.0.0)(PORT=1521)))
 
……
 
SQL> Disconnected from Oracle Database 12c Enterprise Edition Release 12.2.0.1.0 - 64bit Production
#########################
DATABASE IS READY TO USE!
#########################
The following output is now a tail of the alert.log:
Completed: alter pluggable database ORCLPDB1 open
2017-05-20T14:31:25.862061+00:00
ORCLPDB1(3):CREATE SMALLFILE TABLESPACE "USERS" LOGGING  DATAFILE  '/opt/oracle/oradata/ORCLCDB/ORCLPDB1/users01.dbf' SIZE 5M REUSE AUTOEXTEND ON NEXT  1280K MAXSIZE UNLIMITED  EXTENT MANAGEMENT LOCAL  SEGMENT SPACE MANAGEMENT  AUTO
ORCLPDB1(3):Completed: CREATE SMALLFILE TABLESPACE "USERS" LOGGING  DATAFILE  '/opt/oracle/oradata/ORCLCDB/ORCLPDB1/users01.dbf' SIZE 5M REUSE AUTOEXTEND ON NEXT  1280K MAXSIZE UNLIMITED  EXTENT MANAGEMENT LOCAL  SEGMENT SPACE MANAGEMENT  AUTO
ORCLPDB1(3):ALTER DATABASE DEFAULT TABLESPACE "USERS"
ORCLPDB1(3):Completed: ALTER DATABASE DEFAULT TABLESPACE "USERS"
2017-05-20T14:31:26.657295+00:00
ALTER SYSTEM SET control_files='/opt/oracle/oradata/ORCLCDB/control01.ctl' SCOPE=SPFILE;
   ALTER PLUGGABLE DATABASE ORCLPDB1 SAVE STATE
Completed:    ALTER PLUGGABLE DATABASE ORCLPDB1 SAVE STATE
  
2017-05-20T14:41:16.140414+00:00
ORCLPDB1(3):Resize operation completed for file# 10, old size 337920K, new size 358400K
```


## 当然你build好的镜像也是可以push到自己的Docker Hub中，我私人的Docker Hub如下：
https://hub.docker.com/repository/docker/zhenzhenman/oracle12

至于如何操作以及Oracle的CDB和PDB，TableSpace等等，请看下回分解。。。
