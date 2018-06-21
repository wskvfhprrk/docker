#基础镜像
FROM centos:7

#维护人员信息
MAINTAINER hejz "75412985@qq.com"

#设置工作目录，这个命令是用来切换工作目录的，相当于cd命令
WORKDIR /home
RUN mkdir JDK && mkdir Tomcat 

#复制本地conf到/home
ADD conf /home


#解压复制到镜像中的jdk压缩包，完成后删除，RUN命令可以使用 && 将两条命令放到一起，减少镜像的层数
RUN tar zxf /home/jdk-8u171-linux-x64.tar.gz -C /home/JDK && rm -rf /home/jdk-8u171-linux-x64.tar.gz

#设置环境变量
ENV JAVA_HOME /home/JDK/jdk1.8.0_171
ENV PATH $PATH:$JAVA_HOME/bin

#安装tomcat，同JDK
RUN tar zxf /home/apache-tomcat-8.5.31.tar.gz -C /home/Tomcat && rm -rf /home/apache-tomcat-8.5.31.tar.gz

#暴露tomcat的内部端口，如果没有修改tomcat的配置文件的话，默认是8080端口
EXPOSE 8080 22

#启动容器时，执行脚本文件启动Tomcat并持续输出日志，防止容器退出。
ENTRYPOINT /home/Tomcat/apache-tomcat-8.5.31/bin/startup.sh && tail -f /home/Tomcat/apache-tomcat-8.5.31/logs/catalina.out