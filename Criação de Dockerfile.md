# ***Criação de Dockerfile***

## ***Meu primeiro Dockerfile***

​	Esse arquivo é um exemplo de comandos básico da sintaxe do dockerfile.

```dockerfile
FROM debian

LABEL app="Saulo"
ENV SAULO="LINDO"

RUN apt-get update && apt-get install -y stress && apt-get clean

CMD stress --cpu 1 --vm-bytes 64M --vm 1
```

### ***Para fazer o build da imagem do dockerfile:***

```shell
docker image buil -t toskeira:1.0 .
# O parametro -t foi usado para tribuir uma tag a imagem no caso do exemplo acima "Toskeira", no final do comando foi usando o "." indicando que o arquivo dockerfile estava no mesmo diretorio que o comando foi executado.
```

#### ***Executando o minha primeira imagem***

```shell
docker container run -d toskeira:1.0
```

