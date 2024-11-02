# docker 安装mysql
## 适合任何版本
使用的是8.0
^ 5.4版本使用阿里云时不时打不开或丢失自建数据库，不知道什么原因，所以改用8.0

## 安装过程
### 1、运行一个简单的容器
```sh
docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=123456 -d mysql:8.0
```
### 进入 Docker 容器内部
```sh
docker exec -it <容器ID或名称> /bin/bash
```
### 从容器中拷贝出配置文件到宿主机目录中
```sh
docker cp <容器ID或名称>:<容器内路径> ./
```
### 删除运行的mysql容器
```sh
docker rm -f <容器ID或名称>
```
### 运行容器
```sh
docker run --restart=always --name mysql -v /usr/local/docker/mysql:/var/lib/mysql -p 3306:3306 -e MYSQL\_ROOT\_PASSWORD=123456 -d mysql:8.0
```
