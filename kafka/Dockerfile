FROM centos:7

MAINTAINER hejianzhe

#安装JDK
RUN mkdir /var/tmp/jdk
RUN wget --no-check-certificate --no-cookies --header "Cookie: oraclelicense=accept-securebackup-cookie"  -P http://download.oracle.com/otn-pub/java/jdk/8u171-b11/512cd62ec5174c3487ac17c61aaa89e8/jdk-8u171-linux-x64.tar.gz
RUN tar xzf /var/tmp/jdk/jdk-8u171-linux-x64.tar.gz -C /var/tmp/jdk
RUN rm -rf /var/tmp/jdk/jdk-8u171-linux-x64.tar.gz

#设置环境变量
ENV JAVA_HOME /var/tmp/jdk/jdk1.8.0_111
ENV PATH $PATH:$JAVA_HOME/bin

#安装kafka
ENV VERSION=2.12-1.1.0
ADD conf /opt/kafka/
RUN mkdir /opt/kafka
WORKDIR /opt/kafka

RUN curl -sLo - http://mirrors.shu.edu.cn/apache/kafka/1.1.0/kafka_${VERSION}.tgz  | tar zxvf -
RUN mv kafka_${VERSION} kafka
RUN mv server.properties /opt/kafka/kafka/config/zookeeper.properties
RUN mv start.sh /
RUN chmod +750 /start.sh

EXPOSE 2181 9092

CMD "/start.sh"
