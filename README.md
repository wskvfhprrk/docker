常用的docker 命令

### 查询命令

`docker sarch XXX`

### 拉取镜像

`dockker pull XXXX`

查看命令

`docker ps`或 `docker ps -a`
### docker镜像的导入导出（不使用网络）
导出镜像文件
```
docker save -o /路径名/文件名.tar.gz 镜像名称
```
### 删除所有未运行的容器（已经运行的删除不了，未运行的就一起被删除了）
```
docker rm $(sudo docker ps -a -q)
```
### docker删除未使用到的镜像
```
docker image prune -a
docker image prune -a -f 　　#-f强制，不需要确认
```


导入镜像文件
```
docker load < /路径名/文件名.tar.gz
```

#### docker时区同步

` docker run --name <name> -v /etc/localtime:/etc/localtime:ro  .... `

#### mysql数据库

先创建mysql数据卷目录/usr/local/docker/mysql/data

```
mkdir -p /usr/local/docker/mysql/data
mkdir -p /usr/local/docker/mysql/conf
```

docker mysql命令----------密码为123456,端口号为3306 配置文件放在/usr/local/docker/mysql/conf下:conf.d和my.conf

```
docker run --restart=always --name mysql -v \
/usr/local/docker/mysql/data:/var/lib/mysql \
-v /usr/local/docker/mysql/conf:/etc/mysql \
-p 3306:3306 \
-e MYSQL\_ROOT\_PASSWORD=123456 -d mysql:5.7
```
#### windows docker mysql
```
docker run --restart=always --name mysql -p 3306:3306 -v /d/mysql_data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 -d mysql:5.7
```

如果`/my/custom/config-file.cnf`是自定义配置文件的路径和名称，则可以`mysql`像这样启动容器（请注意，此命令中仅使用自定义配置文件的目录路径）：

```
docker run  d --name some-mysql -v /my/custom:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:tag
```

这将启动一个新容器`some-mysql`，其中MySQL实例使用组合的启动设置，`/etc/mysql/my.cnf`并且`/etc/mysql/conf.d/config-file.cnf`后者的设置优先。

#### nexus私服

[官方博客](https://github.com/sonatype/docker-nexus3)

*使用数据卷容器*。由于数据卷是持久的，直到没有容器使用它们，因此可以专门为此目的创建容器。这是推荐的方法。

```
$ docker volume create --name nexus-data
$ docker run -d -p 10081:8081 --name nexus -v nexus-data:/nexus-data sonatype/nexus3
```
#### rabbit
```
docker run --restart=always -d  --name rabbit -p 15672:15672 -p 5672:5672 -e RABBITMQ_DEFAULT_USER=root -e RABBITMQ_DEFAULT_PASS=123456 rabbitmq:3-management
```
#### docker 本地镜像仓库
```
mkdir -p /opt/data/registry  //创建目录
sudo docker run -d -p 5000:5000 -v /opt/data/registry:/var/lib/registry  --name private_registry registry  //启动容器
```
因为Docker从1.3.X之后，与docker registry交互默认使用的是https，然而此处搭建的私有仓库只提供http服务，所以当与私有仓库交互时就会报上面的错误。为了解决这个问题需要在启动docker server时增加启动参数为默认使用http访问。修改docker启动配置文件：

`vi  /usr/lib/systemd/system/docker.service`

找到 ExecStart

`ExecStart=/usr/bin/dockerd  --insecure-registry 192.168.166.186:5000`

#### 重启docker
```
systemctl daemon-reload
systemctl restart docker
```
#### redis
```
docker run --restart=always --name redis -d -p 6379:6379 redis redis-server --appendonly yes
```
#### 建立直播流服务器
```
docker run -d -p 1935:1935 -p 80:80 -v /opt/nginx/html/:/opt/nginx/html/ --rm alfg/nginx-rtmp
```
#### docker容器内部安装软件
```
FROM frolvlad/alpine-oraclejdk8
COPY target/*.jar app.jar
EXPOSE 8080
#使用上海时间为零时区
RUN echo "Asia/Shanghai" > /etc/timezone
#使用使用aplache-linux安装源文件
RUN  apk add php7 --repository http://nl.alpinelinux.org/alpine/edge/testing/
#使用apk add并安装zip
RUN apk add zip
ENTRYPOINT ["java","-jar","/app.jar"]
```

### shadowsocks

```
yum install -y
yum install docker -y
systemctl start docker
docker run -d -p 12345:12345 oddrationale/docker-shadowsocks -s 0.0.0.0 -p 12345 -k 1978106hjz -m aes-256-cfb
docker run -e PASSWORD=1978106hjz -p 8388:8388 -p 8388:8388/udp -d --name shadowsocks shadowsocks/shadowsocks-libev

docker run -d -p 12345:12345 oddrationale/docker-shadowsocks -s 0.0.0.0 -p 12345 -k 1978106hjz -m aes-256-cfb
```

### elasticsearch
创建用户定义的网络（用于连接到连接到同一网络的其他服务（例如，Kibana））
```
docker network create somenetwork
```
```
docker run -d --name elasticsearch --net somenetwork -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" elasticsearch:7.6.2
```
再安装Kibana
```
docker run -d --name kibana --net somenetwork -p 5601:5601 kibana:7.6.2
```
