docker操作命令:
docker build -t jjmf/fastdfs:1.0 .
docker run --name fastdfs -d -p 8888:8888 jjmf/fastdfs:1.0

#需要开启80\22122\23000端口(docker时也要开启这三个端口)
firewall-cmd --zone=public --add-port=80/tcp --permanent 
firewall-cmd --zone=public --add-port=22122/tcp --permanent 
firewall-cmd --zone=public --add-port=23000/tcp --permanent 
#重启防火墙 
systemctl restart firewalld.service 

#client.conf配置文件
connect_timeout = 2
network_timeout = 30
charset = ISO8859-1
http.tracker_http_port = 8080
http.anti_steal_token = no
http.secret_key = FastDFS1234567890
tracker_server = 192.168.1.103:22122 #IP可根据实际情况定,端口不亮

#更改 client.conf\ storage.conf\ mod_fastdfs.conf的IP地址

#启动服务
/etc/init.d/fdfs_trackerd start
/etc/init.d/fdfs_storaged start
/usr/local/nginx/sbin/nginx