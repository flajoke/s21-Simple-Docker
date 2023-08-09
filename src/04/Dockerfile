FROM nginx:latest

COPY main.c home/
COPY nginx.conf etc/nginx/
COPY script.sh home/

RUN apt-get update && apt-get install -y gcc spawn-fcgi libfcgi-dev && chmod +x /home/script.sh

ENTRYPOINT ["bash", "/home/script.sh"]
