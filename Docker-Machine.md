# ***Docker Machine***

## ***Comandos básicos***

### ***Exibindo versão***

```shell
docker-machine version
```

### ***Criando um host virtual (Virtualbox e hyperv)***

```shell
docker-machine create --driver virtualbox <Nome da máquina virtual>
docker-machine create --driver hyperv <Nome da máquina virtual>
```

### ***Listando hosts virtuais***

```shell
docker-machine ls
```

### ***Exibindo variáveis de ambiente de um host***

```shell
docker-machine env <Nome da máquina>
```

### ***Conectando máquina local ao host virtual remoto***

```shell
eval "$(docker-machine env <Nome da máquian>)"
```

### ***Exibindo IP do host virtual***

```shell
docker-machine ip <Nome da máquina>	
```

### ***Conectando ao host virtual***

```shell
docker-machine ssh <Nome da máquina>
```

### ***Exibindo detalhes do host remoto virtual***

```shell
docker-machine inspect <Nome da máquina>
```

### ***Parando, Iniciando e exibindo hosts em execução***

```shell
docker-machine stop <Nome da máquina>
docker-machine start <Nome da máquina>
docker-machine ls
```

### ***Removendo host***

```shell
docker-machine rm linuxtips
```

### ***Retirando as varias de ambiente do host virtual da máquina local***

```shell
eval $(docker-machine env -u)
```

