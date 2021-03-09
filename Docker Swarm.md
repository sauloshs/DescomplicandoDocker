# ***Docker Swarm***

​	Docker Swarm é o orquestrador de container do Docker, com ele podemos subir cluster docker para orquestra os nossos container.

### ***Iniciando um cluster Swarm***

```shell
docker swarm init
```

​	A saída desse comando gera um token que pode ser usado para adicionar outros nós do cluster.

#### ***Caso eu tenha mais de uma interface de rede devo passar qual o IP do meu cluster***

```shell
docker swarm init --advertise-addr 192.168.17.20
```

### ***Promovendo um nó do cluster em manager***

```shell
docker node promote <nome do nó / máquina>
```

### ***Listando nós do cluster***

```shell
docker node ls	
```

### ***Despromovendo um nó do cluster de manager***

```shell
docker node demote <nome do nó / máquina>
```

### ***Exibindo token do manager e worker***

```shell
docker swarm join-token manager

docker swarm join-token worker	
```

### ***Alterando a chave token do cluster***

```shell
docker swarm join-tocken --rotate manager #altear token manager

docker swarm join-token --rotate worker #alterar token worker
```

​	Obs.: A alteração da chave não afeta os nós já ingressado no cluster.

### ***Para remover um nó do cluster***

```shell
docker node demote <nome do nó> # caso ele seja manager
docker node rm -f <nome do nó>
```

### ***Criando um container em vários nós***

```shell
docker service create --name webserver --replicas 3 -p 8080:80 nginx
```

### ***Listando container em que o docker esta em execução***

```shell
docker service ps <nome do container>
```

### ***Manutenção em um nó do cluster***

​	Com o parametro "--availability" podemos mudar o estado do host docker peara três

- active

- pause

- drain

  O modo active torna o nó do cluster apto para receber novos containers.

  O modo pause deixa o estado do nó em pausa e ele não recebera novos containeres provisionado.

  O modo drain desativa o no do cluster e qualquer container que estiver sendo executando nesse nó será desligado e transferido a sua execução para outro.

```shell
docker node update --availability pause <nome do nó>

docker node update --availability active <nome do nó>

docker node update --availability drain <nome do nó>
```

### ***Inspecionando configuração do nó do cluster***

```shell
docker node inspect <nome do nó>
```

### ***Escalando novos container para um container existente em um cluster***

```shell
docker service scale <nome do container>=10 # nesse exemplo escalamos para 10 container 
```

### ***Removendo um container do cluster***

```shell
docker service rm <nome do cotnainer>
```

