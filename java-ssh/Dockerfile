FROM centos:7

#安装openssh-server
RUN yum -y install openssh-server openssh-clients
RUN /usr/sbin/sshd-keygen -A

#变更root密码为123456
RUN echo "root:123456"|chpasswd

#开放窗口的22端口
EXPOSE 22

#运行脚本，启动sshd服务
CMD ["/usr/sbin/sshd", "-D"] 