# docker

## 准备

1. docker 安装
2. 用户级配置，取消 sudo 
3. 创建 jdk php python 镜像
4. 服务启动
5. 镜像仓库
6. 镜像网站修改为国内
7. .dockerignore
8. 发布 image 文件
9. 基于 nexus 的私有 docker 仓库

## dockerfile

sudo docker build -t image_name .

## docker compose

docker-compose up -d

docker-compose start
docker-compose stop
docker-compose down --rmi all
docker-compose exec xx bash

## 参考链接

* [docker入门](http://www.ruanyifeng.com/blog/2018/02/docker-tutorial.html
), by 阮一峰
* [docker微服务](http://www.ruanyifeng.com/blog/2018/02/docker-wordpress-tutorial.html), by 阮一峰