# kong-demo

api gateway use kong, backend use php, nodejs, golang etc.

## versions

    - php 7.1.17
    - node v8.11.3
    - go 1.11
    - kong 0.14.1
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

```bash
docker run --rm \
    --network=kong-net \
    -e "KONG_DATABASE=cassandra" \
    -e "KONG_PG_HOST=kong-database" \
    -e "KONG_CASSANDRA_CONTACT_POINTS=kong-database" \
    kong:latest kong migrations up
```

## links

 - https://github.com/Kong/kong
 - https://getkong.org