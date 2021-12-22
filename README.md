# Development docker loadbalancer

Small docker compose as entrypoint for a localhost docker development. This load balancer has the following service

* Traefik
* PHPMyadmin
* Mailhog

Create a network first, but it is better to create a network per project

```shell
docker network create web 
docker network create itea 
docker network create ecp 
docker network create aeneas 
docker network create pa-portal 
docker network create permit-manager
docker network create solodb-xenics
docker network create solodb-imec
docker network create solodb-equipage
docker network create equipage
docker network create navela
docker network create moonraker

```

Create a certificate via (macOS)

```shell
openssl req \
    -newkey rsa:2048 \
    -x509 \
    -nodes \
    -keyout certs/localhost.key \
    -new \
    -out certs/localhost.pem \
    -subj "/CN=*.docker.localhost" \
    -reqexts SAN \
    -extensions SAN \
    -config <(cat /System/Library/OpenSSL/openssl.cnf \
        <(printf '[SAN]\nsubjectAltName=DNS:*.docker.localhost')) \
    -sha256 \
    -days 3650
```

Create a certificate via (Linux)

```shell
openssl req \
    -newkey rsa:2048 \
    -x509 \
    -nodes \
    -keyout certs/localhost.key \
    -new \
    -out certs/localhost.pem \
    -subj "/CN=*.docker.localhost" \
    -reqexts SAN \
    -extensions SAN \
    -config <(cat /etc/ssl/openssl.cnf \
        <(printf '[SAN]\nsubjectAltName=DNS:*.docker.localhost')) \
    -sha256 \
    -days 3650
```

For linux take the /etc/...openssl.conf Trust the certificate in the host OS so the browser accepts the certificate