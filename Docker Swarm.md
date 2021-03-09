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

