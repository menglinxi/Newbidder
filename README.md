RTB4FREE Bidder
===============

This is the bidder component to the RTB4FREE open source DSP.  The bidder is a JAVA 11 based openRTB bidding system and built on top of Hazelcast, Postgres, Kafka, Mino/S3, Logstash, and Elastic Search. 

The system is Dockerized and allows you to run a DSP totally within your own control. It comes with a React based campaign manager that uses a REST API to 

<b>WARNING: This is not a turnkey DSP.</b><br><br>

<a href="http://rtb4free.com" target="_blank">Directions for using this system is located here</a>

```
docker run -it --network newbidder_default --rm ches/kafka bash
```
更新源文件如下:
deb http://mirrors.aliyun.com/ubuntu/ focal main restricted
deb http://mirrors.aliyun.com/ubuntu/ focal-updates main restricted
deb http://mirrors.aliyun.com/ubuntu/ focal universe
deb http://mirrors.aliyun.com/ubuntu/ focal-updates universe
deb http://mirrors.aliyun.com/ubuntu/ focal multiverse
deb http://mirrors.aliyun.com/ubuntu/ focal-updates multiverse
deb http://mirrors.aliyun.com/ubuntu/ focal-backports main restricted universe multiverse
deb http://cn.archive.ubuntu.com/ubuntu/ focal main restricted
deb http://cn.archive.ubuntu.com/ubuntu/ focal-updates main restricted
deb http://cn.archive.ubuntu.com/ubuntu/ focal universe
deb http://cn.archive.ubuntu.com/ubuntu/ focal-updates universe
deb http://cn.archive.ubuntu.com/ubuntu/ focal multiverse
deb http://cn.archive.ubuntu.com/ubuntu/ focal-updates multiverse
deb http://cn.archive.ubuntu.com/ubuntu/ focal-backports main restricted universe multiverse
deb http://security.ubuntu.com/ubuntu focal-security main restricted
deb http://security.ubuntu.com/ubuntu focal-security universe
deb http://security.ubuntu.com/ubuntu focal-security multiverse

安装jdk17.
![4452443cabba5f0f4ef49d55934ca27](https://user-images.githubusercontent.com/3926945/188259047-6e736dc7-20bf-4f34-ad18-18e1472f9c22.png)

安装nvm,node多版本管理器
安装node v12.13.0+, yarn 1.22.19+
安装maven,版本必须是3.8版本+,不然无法适配jdk17
先部署minio
修改minio.yml里的挂载路径.
![598def00c1a077951cc4b51bbaaad32](https://user-images.githubusercontent.com/3926945/188259086-6a90b426-7b55-4592-b7e4-e0c74b8ca292.jpg)

命令如下
```
启动:docker-compose -f minio.yml up -d
关闭:docker-compose -f minio.yml down
```
http://localhost:9000
然后,创建用户,设置key
![72f4af621e4ded309f21f7e7f0b4476](https://user-images.githubusercontent.com/3926945/188259000-b3903824-dcc2-4f02-a298-7207547ea8d9.jpg)
![d3bf2df594aa357efd3db399b144901](https://user-images.githubusercontent.com/3926945/188259005-a4a3286e-03c3-4bdf-bb50-88e1cff07204.jpg)
![2c18147b47e032748444a9448695b9f](https://user-images.githubusercontent.com/3926945/188259007-efe69c06-267d-48a3-af3d-b694332c34f7.jpg)

修改 Makefile,里的 minio版块.主要是key,serc值.
  
然后make minio
![73621615dec3079c40556076f9e7bfe](https://user-images.githubusercontent.com/3926945/188259070-6753f71a-9035-4217-a1ec-60a326301878.jpg)
再启动 kafka
```
启动docker-compose -f docker-kafka.yml up -d
关闭docker-compose -f docker-kafka.yml down
```

修改数据库挂载路径
![d440eff44216a260218db373fdca139](https://user-images.githubusercontent.com/3926945/188259110-72562792-2c0a-4da1-b5b8-7f1bf1ce4675.jpg)

编译 jar
```
mvn assembly:assembly -DdescriptorId=jar-with-dependencies  -Dmaven.test.skip=true
```
编译react
```
make react
```
创建镜像
```
docker build -t jacamars/newbidder . 
```
启动 数据库
```
docker-compose -f docker-compose-postgres.yml up -d
docker-compose -f docker-compose-postgres.yml down
```

启动镜像
```
docker-compose -f docker-bidder.yml up -d
```













