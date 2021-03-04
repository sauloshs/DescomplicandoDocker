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

### ***Para fazer o build da imagem do  meu primeiro Dockerfile:***

```shell
docker image buil -t toskeira:1.0 .
# O parametro -t foi usado para tribuir uma tag a imagem no caso do exemplo acima "Toskeira", no final do comando foi usando o "." indicando que o arquivo dockerfile estava no mesmo diretorio que o comando foi executado.
```

#### ***Executando o minha primeira imagem***

```shell
docker container run -d toskeira:1.0
```

## ***Dockerfile com apache***

​	Esse Dockerfile mostra como criar uma imagem com o apache

```dockerfile
FROM debian 

RUN apt-get update && apt-get install -y apache2 && apt-get clean 
ENV APACHE_LOCK_DIR="/var/lock"
ENV APACHE_PID_FILE="/var/run/apache2.pid"
ENV APACHE_RUN_USER="www-data"
ENV APACHE_RUN_GROUP="www-data"
ENV APACHE_LOG_DIR="/var/log/apache2"

LABEL description="Webserver"
LABEL version="1.0.0"

VOLUME /var/www/html
EXPOSE 80
```

​	Não é mais necessário usar o parâmetro "apt-get clean"

### ***Para realizar o build do Dockerfile com o apache:***

```shell
docker image build -t meu_apache:1.0.0 .
```

#### ***Executando a imagem meu_apache***

```shell
docker container run -ti meu_apache:1.0.0
```

​	Esse container foi executado mas o apache não foi colocado como processo principal.

## ***Dockerfile com apache no Entrypoint***

```dockerfile
FROM debian 

RUN apt-get update && apt-get install -y apache2 && apt-get clean 
ENV APACHE_LOCK_DIR="/var/lock"
ENV APACHE_PID_FILE="/var/run/apache2.pid"
ENV APACHE_RUN_USER="www-data"
ENV APACHE_RUN_GROUP="www-data"
ENV APACHE_LOG_DIR="/var/log/apache2"

LABEL description="Webserver"
LABEL version="1.0.0"

VOLUME /var/www/html
EXPOSE 80

ENTRYPOINT ["/usr/sbin/apachectl"]
CMD ["-D", "FOREGROUND"]
```

​	O parâmetro -D Foreground é um parâmetro do apache para que ele rode em primeiro plano

### ***Build da imagem apache com Entrypoint***

```shell
docker image build -t meu_apache:2.0.0 .
```

​	Para não utilizar o cache da primeira imagem que foi utilizada colocamos o parâmetro "--no-cache"

```shell
docker image build -t meu_apache:2.0.0 . --no-cache
```

#### ***Executando a imagem***

```shell
docker container run -d -p 8080:80 meu_apache:2.0.0
```

## ***Dockerfile com o comando COPY***

```dockerfile
FROM debian 

RUN apt-get update && apt-get install -y apache2
ENV APACHE_LOCK_DIR="/var/lock"
ENV APACHE_PID_FILE="/var/run/apache2.pid"
ENV APACHE_RUN_USER="www-data"
ENV APACHE_RUN_GROUP="www-data"
ENV APACHE_LOG_DIR="/var/log/apache2"

COPY index.html /var/www/html/

LABEL description="Webserver"
LABEL version="1.0.0"

VOLUME /var/www/html
EXPOSE 80

ENTRYPOINT ["/usr/sbin/apachectl"]
CMD ["-D", "FOREGROUND"]
```

​	O comando COPY vai copiar o arquivo index.html que deve esta dentro da mesma pasta do Dockerfile (Porque o caminho não foi especificado) para dentro do diretório do container /var/www/html

### ***Executando o build da imagem***

```shell
docker image buil -t meu_apache:3.0.0 .
```

#### ***Executando imagem com o Index personalizado***

```shell
docker container run -ti -p 8080:80 meu_apache:3.0.0
# Testando com curl localhost:8080.
```

## ***Dockerfile com o comando ADD e com outro usuário padrão***

```dockerfile
FROM debian 

RUN apt-get update && apt-get install -y apache2
RUN chown -R www-data:www-data /var/lock/ && chown -R www-data:www-data /var/run/ && chown -R www-data:www-data /var/log/
ENV APACHE_LOCK_DIR="/var/lock"
ENV APACHE_PID_FILE="/var/run/apache2.pid"
ENV APACHE_RUN_USER="www-data"
ENV APACHE_RUN_GROUP="www-data"
ENV APACHE_LOG_DIR="/var/log/apache2"

ADD index.html /var/www/html/

LABEL description="Webserver"
LABEL version="1.0.0"
USER www-data
WORKDIR /var/www/html/

VOLUME /var/www/html
EXPOSE 80

ENTRYPOINT ["/usr/sbin/apachectl"]
CMD ["-D", "FOREGROUND"]
```

​	O comando ADD também copia mas, ele tem a possibilidade de copiar arquivos tar para dentro do container com o seu conteúdo extraído. Ele também a possibilidade de copiar arquivos remotos. (Como em sites).

​	Obs: É necessário da alterar o proprietários dos diretórios das variáveis do container 

### ***Build da imagem***

```shell
docker image buil -t meu_apache:4.0.0 .
```

#### ***Executando Imagem com outro usuário***

```shell
docker container run -ti -p 8080:80 meu_apache:4.0.0
```

## ***Dockerfile de uma imagem golang***

​	Foi criado uma aplicação .go no mesmo nível de diretório do Dockerfile.

```go
package main
import "fmt"

func main()  {
		fmt.Println("Saulo Henrique Aprendendo Docker!!")	
}
```

```dockerfile
FROM golang

WORKDIR /app
ADD . /app
RUN go build -o meugo

ENTRYPOINT ./meugo
```

### ***Build da imagem golang***

```shell
docker image build -t meugo:1.0.0 .
```

#### ***Executando imagem golang***

```shell
docker container run -ti meugo:1.0.0
```

​	Essa Imagem fica muito grande...

## ***Dockerfile usando MultiStage***

```dockerfile
FROM golang AS buildando

WORKDIR /app
ADD . /app
RUN go build -o meugo

FROM alpine

WORKDIR /giropops
COPY --from=buildando /app/meugo /giropops

ENTRYPOINT ./meugo
```

​	Usamos a imagem do golang como multistage para a imagem do Alpine que em seguida copiou o conteúdo do diretório da imagem golang para dentro da do Alpine com o parâmetro "--from=<Nome do MultiStage>"

### ***Build da imagem com Multistage***

```shell
docker image build -t meugo:2.0.0 .
```

#### ***Executando a imagem com MultiStage***

```shell
docker container run -ti meugo:2.0.0
```

​	Ao executar o comando docker image ls foi observado que a imagem meugo:2.0.0 ficou consideravelmente menor.

## ***Dockerfile Healthcheck***

```dockerfile
FROM debian 

RUN apt-get update && apt-get install -y apache2
ENV APACHE_LOCK_DIR="/var/lock"
ENV APACHE_PID_FILE="/var/run/apache2.pid"
ENV APACHE_RUN_USER="www-data"
ENV APACHE_RUN_GROUP="www-data"
ENV APACHE_LOG_DIR="/var/log/apache2"

ADD index.html /var/www/html/
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \ 
 CMD curl -f http://localhost/ || exit 1 
LABEL description="Webserver"
LABEL version="1.0.0"

VOLUME /var/www/html
EXPOSE 80

ENTRYPOINT ["/usr/sbin/apachectl"]
CMD ["-D", "FOREGROUND"]
```

### ***Build da imagem com Healthcheck***

```shell
docker image build -t meu_apache:5.0.0 .
```

#### ***Executando imagem com Healthcheck***

```shell
docker container run -d -p 8080:80 meu_apache:5.0.0
```