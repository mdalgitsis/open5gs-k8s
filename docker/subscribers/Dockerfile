FROM ubuntu:22.04

RUN apt-get update && apt-get install -y gnupg2 wget

RUN apt-key update

RUN apt-get update

RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6

RUN echo "deb http://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/3.4 multiverse" | tee /etc/apt/sources.list.d/mongodb-org-3.4.list

RUN echo "deb http://security.ubuntu.com/ubuntu focal-security main" | tee /etc/apt/sources.list.d/focal-security.list

RUN apt-get -y update

RUN apt-get install mongodb-org-shell mongodb-org-server mongodb-org-mongos mongodb-org-tools

RUN apt-get install -y mongodb-mongosh-shared-openssl3 mongodb-org-tools

RUN rm /etc/apt/sources.list.d/focal-security.list