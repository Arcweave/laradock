FROM rust:latest AS builder

WORKDIR /usr/src/myapp

RUN apt clean && apt -y update && apt -y install --no-install-recommends \
    clang && rm -rf /var/lib/apt/lists/*

RUN git clone --depth 1 --branch v2.4.0 https://github.com/RedisJSON/RedisJSON.git

WORKDIR /usr/src/myapp/RedisJSON

RUN cargo build --release

FROM redis:latest

## For security settings uncomment, make the dir, copy conf, and also start with the conf, to use it
RUN mkdir -p /etc/redis/modules
COPY redis.conf /etc/redis/redis.conf
COPY --from=builder /usr/src/myapp/RedisJSON/target/release/librejson.so /etc/redis/modules/librejson.so

VOLUME /data

EXPOSE 6379

CMD ["redis-server", "/etc/redis/redis.conf"]
# CMD ["redis-server"]
