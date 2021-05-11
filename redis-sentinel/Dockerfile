FROM redis:latest

LABEL maintainer="Mahmoud Zalt <mahmoud@zalt.me>"

## For security settings uncomment, make the dir, copy conf, and also start with the conf, to use it
RUN mkdir -p /usr/local/etc/redis
COPY sentinel.conf /usr/local/etc/redis/sentinel.conf

VOLUME /data

EXPOSE 26379

CMD ["redis-sentinel", "/usr/local/etc/redis/sentinel.conf"]