## select base os
FROM centos:7

COPY nginx.repo /etc/yum.repos.d/nginx.repo

## user infomation
ENV USERNAME="admin"
ENV PASSWORD="password"

## install phase
RUN yum -y install nginx
RUN yum -y install openssl

## basic authentication settings
COPY default.conf /etc/nginx/conf.d/default.conf
RUN echo "${USERNAME}:$(openssl passwd -apr1 ${PASSWORD})" >> /etc/nginx/.htpasswd

## container's port
EXPOSE 80

## run-time commands
CMD ["/usr/sbin/nginx", "-g", "daemon off;"]