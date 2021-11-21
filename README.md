# Development docker loadbalancer

Small docker compose as entrypoint for a localhost docker development. This load balancer has the following
service

 * Traefik
 * PHPMyadmin
 * Mailhog

Create a network first

```shell
docker network create web
```

Create a certificate via

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

For linux take the /etc/...openssl.conf
Trust the certificate in the host OS so the browser accepts the certificate