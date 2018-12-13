常用的docker 命令

### 查询命令

`docker sarch XXX`

### 拉取镜像

`dockker pull XXXX`

查看命令

`docker ps`或 `docker ps -a`



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



#### nexus私服

[官方博客](https://github.com/sonatype/docker-nexus3)

*使用数据卷容器*。由于数据卷是持久的，直到没有容器使用它们，因此可以专门为此目的创建容器。这是推荐的方法。

```
$ docker volume create --name nexus-data
$ docker run -d -p 10081:8081 --name nexus -v nexus-data:/nexus-data sonatype/nexus3
```

