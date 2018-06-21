FROM centos:7

RUN yum -y update

RUN yum install -y make gcc net-tools build-essential libtool libpcre3 libpcre3-dev zlib1g-dev openssl libeventy install gcc-c++ pcre pcre-devel zlib zlib-devel openssl openssl-devel unzip


MAINTAINER hejianzhe

ADD soft /usr/local/src/

WORKDIR /usr/local/src/

RUN unzip libfastcommon-master.zip && \
	rm -f libfastcommon-master.zip && \
	cd libfastcommon-master && \
	./make.sh && \
	./make.sh install && \
	#安装fastdfs	
	cd .. && \
	unzip fastdfs-master.zip && \
	rm -f fastdfs-master.zip && \
	cd fastdfs-master && \
	./make.sh && \
	./make.sh install && \
	#添加fastdfs配置文件
	cd .. && \
	rm -rf /etc/fdfs/* && \
	mv tracker.conf /etc/fdfs/ && \
	mv storage.conf /etc/fdfs/ && \
	mv client.conf /etc/fdfs/ && \
	mv -f /usr/local/src/fdfs_trackerd /etc/init.d/fdfs_trackerd && \
	mv -f /usr/local/src/fdfs_storaged /etc/init.d/fdfs_storaged && \
	mv http.conf /etc/fdfs/ && \
	mv mime.types /etc/fdfs/ && \
	mv mod_fastdfs.conf /etc/fdfs/ && \
	#创建数据目录
	mkdir -p /home/fastdfs && \	
	mkdir -p /home/fastdfs/data/M00 && \
	#安装配置nginx
	cd /usr/local/src/ && \
	unzip fastdfs-nginx-module-master.zip && \
	rm -f fastdfs-nginx-module-master.zip && \
	tar -zxvf nginx-1.12.2.tar.gz && \
	rm -f nginx-1.12.2.tar.gz && \
	cd nginx-1.12.2 && \
	./configure --prefix=/usr/local/nginx --add-module=../fastdfs-nginx-module-master/src && \
	make && \
	make install && \
	#配置nginx文件	
	cd .. &&\
	mv -f nginx.conf /usr/local/nginx/conf/ && \
	chmod +x /usr/local/src/start.sh
	
EXPOSE 80
ENTRYPOINT /usr/local/src/start.sh  
	
	
	


