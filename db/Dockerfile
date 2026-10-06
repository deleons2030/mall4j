FROM mysql:8.0.32

MAINTAINER opgames(opgames.cn@gmail.com)

RUN ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime

COPY ./db/yami_shop.sql /docker-entrypoint-initdb.d
