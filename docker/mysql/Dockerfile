FROM mysql:8.0

ENV DEBIAN_FRONTEND noninteractive

# CST zone
#ENV TZ=UTC
#RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN sed -i "s@deb.debian.org@mirrors.aliyun.com@g" /etc/apt/sources.list
RUN apt-get clean \
    && apt-get update \
    && apt-get install -y vim iputils-ping

COPY my.cnf /etc/mysql/my.cnf
