---
title: Docker创建镜像并上传到Docker Hub
date: 2020-08-20 20:52:41
comment: true
categories: Docker
tags:
- Docker
- Docker Hub
---

介绍如何实现自己build一个Docker image，然后push到自己到Docker Hub中的过程。
<!--more-->

# 准备工作
需要自己注册一个Docker Hub的账号。
URL： https://hub.docker.com/
![注册/登录](signup.jpg)

# Step 1，启动Docker
启动docker，保持其running状态

# Step 2，login
 **新建一个命令行窗口**
 ```bash
docker login
 ```
 ![Login](login.jpg)

# Step 3 查看容器

```bash
docker ps -a
```
 ![Login](containers.jpg)

# Step 4 选择要生成镜像的容器ID（CONTAINER ID)
```bash
docker commit #{CONTAINER ID} yourImageName
```
# Step 5 查看生产的镜像
```bash
docker images
```
# Step 6 修改镜像指向自己的Docker Hub
```bash
docker tag #{IMAGE ID} dockerHub用户名/dockerHub仓库名:tag
```
 ![Images](list.jpg)
命令行中用户名和仓库名，对应自己Docker Hub的用户名及仓库名，最后冒号后的tag是你要打的tag，如下图中显示的样子，当然随便写
![Tags](imagetags.jpg)

# Step 7 push镜像至Docker Hub
```bash
docker push dockerHub用户名/dockerHub仓库名:tag标签
```
tag标签便是上一步中打的tag

**经过如上步骤，便可以在自己的Docker Hub上看到刚刚push上来的镜像了**