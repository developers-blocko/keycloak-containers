FROM jboss/base-jdk:8

ENV KEYCLOAK_VERSION 7.0.0
ENV JDBC_POSTGRES_VERSION 42.2.5
ENV JDBC_MYSQL_VERSION 5.1.46
ENV JDBC_MARIADB_VERSION 2.2.3
ENV JDBC_MSSQL_VERSION 7.4.1.jre8

ENV LAUNCH_JBOSS_IN_BACKGROUND 1
ENV PROXY_ADDRESS_FORWARDING false
ENV JBOSS_HOME /opt/jboss/keycloak
ENV LANG en_US.UTF-8

COPY $KEYCLOAK_DIST_FILE /org/jboss

ARG GIT_REPO
ARG GIT_BRANCH
ARG KEYCLOAK_DIST=https://downloads.jboss.org/keycloak/$KEYCLOAK_VERSION/keycloak-$KEYCLOAK_VERSION.tar.gz
ARG KEYCLOAK_DIST_FILE
ARG KEYCLOAK_DIST_FILE_NAME=keycloak-$KEYCLOAK_VERSION.tar.gz

USER root

RUN yum install -y wget
RUN wget http://mirror.centos.org/centos/7/extras/x86_64/Packages/epel-release-7-11.noarch.rpm
RUN rpm -Uvh epel-release-7-11.noarch.rpm
RUN yum-config-manager --disable epel
RUN yum update -y && yum install -y git && yum install -y jq openssl which && yum clean all

ADD tools /opt/jboss/tools
RUN /opt/jboss/tools/build-keycloak.sh

USER 1000

EXPOSE 8080
EXPOSE 8443

ENTRYPOINT [ "/opt/jboss/tools/docker-entrypoint.sh" ]

CMD ["-b", "0.0.0.0"]
