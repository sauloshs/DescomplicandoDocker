# ***Docker Secrets***

## ***Trabalhando com docker secrets***

### ***Criando um docker secrets***

```shell
# Enviando atraves de um comando echo
echo -n "Saulo Henrique Secrect" | docker secret create <nome da secret> -
# Atraves de um arquivo criado
docker secret create <nome da secret> <nome do arquivo>	
```

### ***Inspecionando uma secret***

```shell
 docker secret inspect <nome da secret>	
```

### ***Criando um cluster swarm com uma secret***

```shell
 docker service create --name <nome do container> -p 8080:80 --secret <nome da secret> nginx
```

###  ***Manipulando uma secret de um cluster swarm***

```shell
docker service update --secret-add <nome da secret> nginx
```

#### ***Removendo***

```sh
docker service update --secret-rm <nome da secret> nginx
```

