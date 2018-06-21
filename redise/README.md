#创建路径
sudo mkdir -p /usr/local/docker/redis

#复制文件夹所有东西到/usr/local/docker/redis

#使用Dockerfile建立新的镜象jjmf/redis:1.0,启动后的密码为123456,如果需要更改需要更改redis.conf,并开启远程访问端口
docker build -t jjmf/redis:1.0 .

#以redis命名的映射为80端口的服务器启动
docker run --name redis -d -p 6379:6379 jjmf/redis:1.0


#问题汇总
1没有密码,更改redis.conf:解决办法：去掉requirepass前面的#号 后面为password
2不能远程访问:一是更改redis.conf:解决办法：注释掉bind 127.0.0.1可以使所有的ip访问redis;二是linux防火墙没有打开6379端口


#添加防火墙(阿里云要先启动:https://help.aliyun.com/knowledge_detail/41317.html)
systemctl start firewalld       ##阿里云要先启动Firewall---阿里云默认没有启动Firewall
firewall-cmd --zone=public --add-port=6379/tcp --permanent  
#重启
systemctl restart firewalld.service


#直接启动建立redis无密码,一般不使用,添加密码里需要到设置
docker run -p 6379:6379 --name redis -d redis 
#linux重启后要修改redis密码
CONFIG SET requirepass "123456"
AUTH 123456