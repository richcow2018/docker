FROM ubuntu:16.04

MAINTAINER jimmy.chu@asiainspection.com

ENV TOMCAT_VERSION 7.0.52

# Set locales
# Set the locale
RUN apt-get clean && apt-get update && apt-get install -y locales
RUN locale-gen en_US.UTF-8
ENV LANG en_US.UTF-8
ENV LC_CTYPE en_US.UTF-8

# Fix sh
RUN rm /bin/sh && ln -s /bin/bash /bin/sh

# Install dependencies
RUN apt-get update && \
apt-get install -y git build-essential curl wget software-properties-common

# Install JDK 8
RUN \
echo oracle-java8-installer shared/accepted-oracle-license-v1-1 select true | debconf-set-selections && \
add-apt-repository -y ppa:webupd8team/java && \
apt-get update && \
apt-get install -y oracle-java8-installer wget unzip tar && \
rm -rf /var/lib/apt/lists/* && \
rm -rf /var/cache/oracle-jdk8-installer

# Define commonly used JAVA_HOME variable
ENV JAVA_HOME /usr/lib/jvm/java-8-oracle

# Get Tomcat
# RUN wget --quiet --no-cookies http://apache.rediris.es/tomcat/tomcat-7/v${TOMCAT_VERSION}/bin/apache-tomcat-${TOMCAT_VERSION}.tar.gz -O /tmp/tomcat.tgz && \
RUN wget --quiet --no-cookies http://archive.apache.org/dist/tomcat/tomcat-7/v7.0.52/bin/apache-tomcat-7.0.52.tar.gz -O /tmp/tomcat.tgz && \
tar xzvf /tmp/tomcat.tgz -C /usr/local/ && \
mv /usr/local/apache-tomcat-${TOMCAT_VERSION} /usr/local/tomcat && \
rm /tmp/tomcat.tgz && \
rm -rf /usr/local/tomcat/webapps/examples && \
rm -rf /usr/local/tomcat/webapps/docs && \
rm -rf /usr/local/tomcat/webapps/ROOT

# SSH installation
# make sure the package repository is up to date
RUN apt-get update
RUN apt-get install -y openssh-server
RUN mkdir /var/run/sshd
RUN echo 'root:Pass1230' |chpasswd


# Add admin/admin user
ADD tomcat-users.xml /usr/local/tomcat/conf/

ENV CATALINA_HOME /usr/local/tomcat
ENV PATH $PATH:$CATALINA_HOME/bin

EXPOSE 8080
# EXPOSE 8009
EXPOSE 22
VOLUME "/usr/local/tomcat/webapps"
WORKDIR /usr/local/tomcat

# Launch Tomcat
CMD ["/usr/local/tomcat/bin/catalina.sh", "run"]

# Launch SSH
 CMD ["/usr/sbin/sshd", "-D"]
