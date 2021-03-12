FROM redis:6.2.1-alpine

ENV SENTINEL_QUORUM 2
ENV SENTINEL_DOWN_AFTER 30000
ENV SENTINEL_FAILOVER 180000

RUN mkdir -p /redis

WORKDIR /redis

COPY sentinel.conf .
COPY entrypoint.sh /usr/local/bin/

RUN chown redis:redis /redis/* && \
    chmod +x /usr/local/bin/entrypoint.sh

EXPOSE 26379

ENTRYPOINT ["entrypoint.sh"]