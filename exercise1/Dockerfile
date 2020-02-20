FROM debian:jessie

WORKDIR /root/aescrypt

RUN apt-get update && apt-get install -y wget make gcc
RUN wget -O aescrypt-3.14.tgz https://www.aescrypt.com/download/v3/linux/aescrypt-3.14.tgz
RUN tar xf aescrypt-3.14.tgz
RUN cd ./aescrypt-3.14/src && ls && make && chmod +x aescrypt && cp aescrypt /usr/local/bin
