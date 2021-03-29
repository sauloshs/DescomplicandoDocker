# ***Docker compose***

​	O padrão da extensão de um arquivo compose é .yml.

## ***Arquivo de configuração***

```yaml
version: "3.8"
services: 
  web:
    image: nginx
    deploy:
      replicas: 5
      resources:
        limits:
          cpus: "0.1"
          memory: 50M
      restart_policy:
        condition: on-failure
    ports: 
      - "8080:80"
    networks: 
      - webserver
networks:
  webserver:
```

​	Este exemplo cria uma stack com um service rodando o nginx

### ***Deploy de uma arquivo compose***

```shell
docker stack deploy -c docker-compose.yml <nome da stack>
```

### ***Listando todas as stacks***

```shell
docker stack ls
```

### ***Listando quais o serviços fazem parta da stack***

```shell
docker stack services <nom da stack>	
```

### ***Removendo uma stack compose***

```shell
docker stack rm <nome da stack>	
```

