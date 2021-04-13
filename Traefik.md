# ***Traefik***

​	Resumidamente o traefik funciona como um proxy reverso para ambientes em container e outros.

## ***Arquivo Yaml para provisionar um service Traefik em um cluster swarm***

```yaml
version: '3'

services:
  reverse-proxy:
    image: traefik:v2.0.2
    labels:
    - "traefik.enable=true"
    - "traefik.http.routers.http-catchall.rule=hostregexp(`{host:.+}`)"
    - "traefik.http.routers.http-catchall.entrypoints=web"
    - "traefik.http.routers.http-catchall.middlewares=redirect-to-https@docker"
    - "traefik.http.middlewares.redirect-to-https.redirectscheme.scheme=https"
    command:
      - "--providers.docker.endpoint=unix:///var/run/docker.sock"
      - "--providers.docker.swarmMode=true"
      - "--providers.docker.exposedbydefault=false"
      - "--providers.docker.network=traefik-public"
      - "--entrypoints.web.address=:80"
      - "--entrypoints.websecure.address=:443"
      - "--certificatesresolvers.letsencryptresolver.acme.httpchallenge=true"
      - "--certificatesresolvers.letsencryptresolver.acme.httpchallenge.entrypoint=web"
      - "--certificatesresolvers.letsencryptresolver.acme.email=jeferson@linuxtips.com.br"
      - "--certificatesresolvers.letsencryptresolver.acme.storage=/letsencrypt/acme.json"
      - "--api.insecure"
      - "--api.dashboard=true"
    ports:
      - 80:80
      - 443:443
      - 8080:8080
    volumes:
      # So that Traefik can listen to the Docker events
      - /var/run/docker.sock:/var/run/docker.sock:ro
      - traefik-certificates:/letsencrypt
    networks:
      - traefik-public
    deploy:
      placement:
        constraints:
          - node.role == manager

volumes:
  traefik-certificates:

networks:
  traefik-public:
    external: true
```

​	No exemplo acima temos um service do Traefik pronto para aceitar requisições na porta 80, 443 e 8080. A ultima que server para acesso ao Dashboard do Traefik. Nesse exemplo as requisições https já são validadas através da entidade certificatória Letsencrypt.

## ***Exemplo de service provisionado no cluster para rodar atrás do Traefik***

```yaml
version: '3'
services:
  loja:
    image: linuxtips/nginx-prometheus-exporter:1.0.0
    networks:
     - traefik-public
    deploy:
      labels:
        - "traefik.enable=true"
        - "traefik.http.routers.loja.rule=Host(`loja.biqueiranerd.com.br`)"
        - "traefik.http.routers.loja.entrypoints=websecure"
        - "traefik.http.routers.loja.tls.certresolver=letsencryptresolver"
        - "traefik.http.services.loja.loadbalancer.server.port=80"
networks:
  traefik-public:
    external: true
```

​	No exemplo acima temos uma aplicação que recebe as requisições vindas do Traefik.