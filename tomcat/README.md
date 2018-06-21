
#建立tomcat docker目录
sudo mkdir -p /usr/local/docker/tomcat

#复制docker所需的文件到些目录下

#
docker build -t jjmf/tomcat:1.0 .


docker run --name tomcat8080 -d -p 8080:8080 jjmf/tomcat:1.0
docker run --name tomcat8081 -d -p 8081:8080 jjmf/tomcat:1.0


#问题:
#启动tomcat docker 后不能够访问:看linux端口是否打开
#添加防火墙
firewall-cmd --zone=public --add-port=8080/tcp --permanent  
#重启
systemctl restart firewalld.service