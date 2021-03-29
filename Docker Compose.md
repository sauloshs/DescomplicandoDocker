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

## ***Arquivo docker compose com duas stacks***

```yaml
version: "3.8"
services: 
  db:
    image: mysql:5.7
    volumes: 
      - db_data:/var/lib/mysql
    environment: 
      MYSQL_ROOT_PASSWORD: somewordpress
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: wordpress

  wordpress:
    depends_on: 
      - db
    image: wordpress:latest
    ports: 
      - "8080:80"
    environment: 
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: wordpress
volumes:
  db_data:
```

## ***Mais exemplo de docker compose***

```shell
version: "3.8"
services: 
  web:
    image: nginx
    deploy:
      placement:
        constraints:
          - node.labels.dc == UK
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

  visualizer:
    image: dockersamples/visualizer:stable
    ports: 
     - "8888:8080"    
    volumes: 
      - "/var/run/docker.sock:/var/run/docker.sock"
    deploy:
      placement:
        constraints: [node.role == manager]
    networks: 
      - webserver 
networks:
  webserver:
```

​	Essa Stack configura o visualizer que é uma interface gráfica onde podemos visualizar onde esta rodando os container/services do nosso cluster.