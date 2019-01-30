常用的docker 命令

### 查询命令

`docker sarch XXX`

### 拉取镜像

`dockker pull XXXX`

查看命令

`docker ps`或 `docker ps -a`

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
docker run --name mysql -v \
/usr/local/docker/mysql/data:/var/lib/mysql \
-v /usr/local/docker/mysql/conf:/etc/mysql \
-p 3306:3306 \
-e MYSQL\_ROOT\_PASSWORD=123456 -d mysql:5.7
```

```
docker run --name mysql -v /f/mysql_data:/var/lib/mysql -p 3306:3306 -e MYSQL\_ROOT\_PASSWORD=123456 -d mysql:5.7
```

如果`/my/custom/config-file.cnf`是自定义配置文件的路径和名称，则可以`mysql`像这样启动容器（请注意，此命令中仅使用自定义配置文件的目录路径）：

```
docker run --name some-mysql -v /my/custom:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:tag
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
docker run -d  --name rabbit -p 15672:15672 -p 5672:5672 -e RABBITMQ_DEFAULT_USER=root -e RABBITMQ_DEFAULT_PASS=123456 rabbitmq:3-management
```
#### docker 本地镜像仓库
```
mkdir -p /opt/data/registry  //创建目录
sudo docker run -d -p 5000:5000 -v /opt/data/registry:/var/lib/registry 
      --name private_registry registry  //启动容器
```
