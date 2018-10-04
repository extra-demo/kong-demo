# kong-demo

api gateway use kong, backend use php, nodejs, golang etc.

## versions

    - php 7.1.17
    - node v8.11.3
    - go 1.11
    - kong 0.14.1 （kong:0.14.1-alpine）
    - kong database (cassandra:3)
    - kong dashboard

## step by step

### install use docker

1. kong-network

```bash
  docker network create kong-net
```

2. kong-database

```bash
    docker run -d --name kong-database \
                --network=kong-net \
                -p 9042:9042 \
                cassandra:3
```

3. prepare database

wait until cassandra start up and then execute this cmd.

```bash
docker run --rm \
    --network=kong-net \
    -e "KONG_DATABASE=cassandra" \
    -e "KONG_PG_HOST=kong-database" \
    -e "KONG_CASSANDRA_CONTACT_POINTS=kong-database" \
    kong:0.14.1-alpine kong migrations up
```

4. start kong

```bash
 docker run -d --name kong \
    --network=kong-net \
    -e "KONG_DATABASE=cassandra" \
    -e "KONG_PG_HOST=kong-database" \
    -e "KONG_CASSANDRA_CONTACT_POINTS=kong-database" \
    -e "KONG_PROXY_ACCESS_LOG=/dev/stdout" \
    -e "KONG_ADMIN_ACCESS_LOG=/dev/stdout" \
    -e "KONG_PROXY_ERROR_LOG=/dev/stderr" \
    -e "KONG_ADMIN_ERROR_LOG=/dev/stderr" \
    -e "KONG_ADMIN_LISTEN=0.0.0.0:8001, 0.0.0.0:8444 ssl" \
    -p 80:8000 \
    -p 8443:8443 \
    -p 8001:8001 \
    -p 8444:8444 \
    kong:0.14.1-alpine
```

## links

 - https://github.com/Kong/kong
 - https://getkong.org