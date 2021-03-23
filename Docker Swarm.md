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

### ***Listando container em cluster***

```shell
docker service ls
```

### ***Listando o nó em que o container esta em execução***

```shell
docker service ps <nome do container>
```

### ***Inspecionando um container em cluster***

```shell
docker service inspect <nome do container> 

# também pode ser utilizado o parametro "--pretty"

docker service inspect <nome do container> --pretty
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

### ***Analisando o log de todos os container em cluster***

```shell
docker service logs -f <nome do container>
```

## ***Volumes em cluster***

### ***Montando um volume em container em cluster***

​	Para realizar esse procedimento será necessário criar o volume em um dos nós do cluster. 

```shell
docker service create --name <nome do container> --replicas 3 -p 8080:80 --mount type=volume,src=<nome do volume>,dst=/usr/share/nginx/html nginx
```

​	Com o procedimento a cima o volume é criado em todos os nós do cluster. Porem arquivos criados em um deles não é sincronizado em todos. Para isso utilizamos o procedimento abaixo.

```shell
# Passos para configuração dos volumes sincronizados.
# Em um dos nós instale "nfs-server"
apt install nfs-server
# Em Seguida configura a pasrta compartilhada no diretorio 
mkdir -p /opt/site/_data
nano /etc/exports
/opt/site *(rw,sync,subtree_check)
# Copiando algo para o diretorio que sera compartilhado entre os nós.
echo "teste" > /opt/site/_data/index.html
# devemos remover a pasta "_data" do volume criado no nó principal para ser substituida para um link simbolico da pasta que criamos anteriomente.
rm -rf /var/lib/docker/volumes/<nome do volume>/_data/
ln -s /opt/site/_data /var/lib/docker/volumes/<nome do volume>/
# validando as configurações de compartilhamento
exportfs -ar

# Nos outros nó devemos instalar "nfs_common"
apt install nfs-common
# verificar o que esta compartilhado no outro nó
showmount -r <ip do primeiro host>
# Montando a unidade
mount <Ip do primeiro host>:/opt/site /var/lib/docker/volumes/<nome do volume>/
```

## ***Redes em cluster***

### ***Criação de uma rede Overlay***

```shell
docker network create -d overlay <nome da rede>
```

### ***Listando redes***

```shell
docker network ls
```

### ***Criando um container service em uma rede especifica***

```shell
docker service create --name <nome do container> -p 8888:80 --network <nome da rede> nginx
```

### ***Adicionando uma rede especifica a um serviço Swarm em execução***

```shell
docker service update --network-add <nome da rede> <nome do container/serviço swarm>
```

​	Todos os containers que pertencem a mesma rede podem se comunicar através do nome do serviço.