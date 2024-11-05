# docker 安装mysql
## 适合任何版本
使用的是8.0
^ 5.4版本使用阿里云时不时打不开或丢失自建数据库，不知道什么原因，所以改用8.0

## 安装过程
### 1、运行一个简单的容器
```sh
docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=123456 -d mysql:8.0
```
### 进入 Docker 容器内部查看mysql配置（可省略）
```sh
docker exec -it some-mysql /bin/bash
```
配置在目录`/var/lib/mysql`下，退出容器进入宿主机使用`exit`指令即可
### 从容器中拷贝出配置文件到宿主机目录中
进入宿主机目录`/usr/local/docker/`中进行拷贝
```sh
docker cp some-mysql:/var/lib/mysql ./
```
### 删除运行的mysql容器
```sh
docker rm -f  some-mysql
```
### 运行容器
```sh
docker run --restart=always --name mysql -v /usr/local/docker/mysql:/var/lib/mysql -p 3306:3306 -e MYSQL\_ROOT\_PASSWORD=123456 -e MYSQL_ROOT_HOST=% -d mysql:8.0
```
## 自动安装sh脚本
```sh
#!/bin/bash

# 定义变量
MYSQL_ROOT_PASSWORD="123456"
CONTAINER_NAME="some-mysql"
HOST_DIR="/usr/local/docker/mysql"
MYSQL_VERSION="8.0"

# 1. 运行一个简单的 MySQL 容器
echo "启动 MySQL 容器..."
docker run --name $CONTAINER_NAME -e MYSQL_ROOT_PASSWORD=$MYSQL_ROOT_PASSWORD -d mysql:$MYSQL_VERSION

# 可选: 进入 Docker 容器内部查看 MySQL 配置
# echo "进入容器查看 MySQL 配置..."
 docker exec -it $CONTAINER_NAME /bin/bash

# 2. 从容器中拷贝出配置文件到宿主机目录中
echo "拷贝 MySQL 配置文件到宿主机..."
docker cp $CONTAINER_NAME:/var/lib/mysql $HOST_DIR

# 3. 删除运行的 MySQL 容器
echo "删除 MySQL 容器..."
docker rm -f $CONTAINER_NAME

# 4. 运行容器
echo "重新启动 MySQL 容器..."
docker run --restart=always --name mysql -v $HOST_DIR:/var/lib/mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=$MYSQL_ROOT_PASSWORD -e MYSQL_ROOT_HOST=% -d mysql:$MYSQL_VERSION

echo "MySQL 容器启动完成！"

```
