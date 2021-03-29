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

### ***Exemplo de stack completa do docker compose***

```yaml
version: "3.8"
services: 
  redis:
    image: redis:alpine
    ports: 
      - "6379"
    networks: 
      - frontend
    deploy:
      replicas: 2
      update_config:
        parallelism: 1
        delay: 10s
        order: start-first
      rollback_config:
        parallelism: 1
        delay: 10s
        failure_action: continue
        monitor: 60s
        order: stop-first
      restart_policy:
        condition: on-failure
  db:
    image: postgres:9.4
    volumes: 
      - db_data:/var/lib/postgresql/db_data
    networks: 
      - backend
    deploy:
      placement:
        constraints: [node.role == manager]
  vote:
    image: dockersamples/examplevotingapp_vote:before
    ports: 
      - 5000:80
    networks: 
      - frontend
    depends_on: 
      - redis
    deploy:
      replicas: 2
      update_config:
        parallelism: 2
      restart_policy:
        condition: on-failure
  result:
    image: dockersamples/examplevotingapp_result:before
    ports: 
      - "5001:80"
    networks: 
      - backend
    depends_on: 
      - db
    deploy:
      replicas: 1
      update_config:
        parallelism: 1
        delay: 10s
      restart_policy:
        condition: on-failure
  worker:
    image: dockersamples/examplevotingapp_worker
    networks: 
      - frontend
      - backend
    deploy:
      mode: replicated
      replicas: 1
      labels: [APP=VOTING]
      restart_policy:
        condition: on-failure
        delay: 10s
        max_attempts: 3
        window: 120s
      placement:
        constraints: [node.role == manager]
  visualizer:
    image: dockersamples/visualizer:stable
    ports: 
      - "8080:8080"
    stop_grace_period: 1m30s
    volumes:
      - "/var/run/docker.sock:/var/run/docker.sock"
    deploy:
      placement:
        constraints: [node.role == manager]
networks: 
  frontend:
  backend:
volumes:
  db_data:
```

