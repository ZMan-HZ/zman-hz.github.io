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
记录在使用了11g以上版本oracle时踩过的坑
<!--more-->
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