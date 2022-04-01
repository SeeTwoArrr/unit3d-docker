# syntax=docker/dockerfile:1

ARG nginx_version

FROM nginx:${nginx_version}

COPY files/unit3d.conf /etc/nginx/conf.d/unit3d.conf

EXPOSE 80