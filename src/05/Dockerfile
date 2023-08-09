FROM nginx:latest
ARG NAME=nginx

COPY main.c home/
COPY nginx.conf etc/nginx/
COPY script.sh home/

RUN apt-get update && \
    apt-get install -y gcc spawn-fcgi libfcgi-dev && \
    rm -rf /var/lib/apt/lists && \
    chmod +x /home/script.sh && \
    chown 755 usr/bin/umount && \
    chown 755 usr/bin/newgrp && \
    chown 755 usr/bin/expiry && \
    chown 755 usr/bin/gpasswd && \
    chown 755 usr/bin/chage && \
    chown 755 usr/bin/passwd && \
    chown 755 usr/sbin/unix_chkpwd && \
    chown 755 usr/bin/su && \
    chown 755 usr/bin/mount && \
    chown 755 usr/bin/wall && \
    chown 755 usr/bin/chfn && \
    chown 755 usr/bin/chsh && \
    touch /var/run/nginx.pid && \
    chown -R $NAME:$NAME /var/run/nginx.pid && \
    chown -R $NAME:$NAME /var/cache/nginx && \
    chmod -R 755 /var/log/nginx

USER $NAME
HEALTHCHECK NONE
ENTRYPOINT ["bash", "/home/script.sh"]
