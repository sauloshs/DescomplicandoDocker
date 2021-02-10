# ***Trabalhando com volumes***

​	Sempre que criamos um container e depois paramos um container o dados desse container é perdido, para que isso não aconteça podemos utilizar os volumes.
​	Volume é uma forma de colocar o file sistem dentro do container.

## ***Tipos de volumes***

​	Quando vamos criar um volume esse pode ser criado de duas maneiras, uma seria do tipo bind que é quando já temos o diretório no nosso host a outra seria do tipo volume que não especificamos o caminho do diretorio e o docker cria um volume com um nome aleatório na pasta /var/lib/docker/volumes.

### ***Montando um volume do tipo Bind***

```shell
docker container run -ti --mount type=bing,src=/opt/giropops,dst=/goiropops debian
```

#### ***Montando um volume do tipo bind somente leitura***

```shell
docker container run -ti --mount type=bing,src=/opt/giropops,dst=/goiropops,ro debian
# O parametro "ro" foi utilizado para representar que o volume é somente leitura.
```

### ***Montando um volume do tipo Volume***

#### ***Listando volumes***

```shell
docker volume ls
```

#### ***Criando volumes***

```shell
docker volume create data
```

#### ***Inspecionando volumes***

```shell
docker volume inspect data
```

#### ***Adicionando um volume a um container***

```shell
docker container run -ti --mount type=volume,src=data,dst=/goiropops debian
```

#### ***Removendo volume***

```shell
docker volume rm <Nome do volume>
```

### ***Comando Prune***

​	Esse comando serve para apagar todos os volumes que não estejam sendo executado por pelo menos um container. Esse comando deve ser usado com cuidado, pois pode haver dados dentro do volume que não pode ser removido.

```shell
docker volume prune
```

### ***Data-Only Container***

​	Esse container era usado antigamente para armazenar dados de outro container. Onde primeiro era criado um container com um volume montado e em seguida criava um outro container com o volume apontando para o container data-only. 

​	Usando a sintaxe antiga:

```shell
docker container create -v /data --name dbdados centos
#foi montato no /data porque vamos usar uma máquian postgresql e esse é o diretorio padrão do PostgreSQL
```

O primeiro container não fica em execução, ela só foi criada.

```shell
docker run -d -p 5432:5432 --name pgsql1 --volumes-from dbdados -e POSTGRESQL_USER=docker -e POSTGRESQL_PASS=docker -e POSTGRESQL_DB=docker kamui/postgresql
```

O segundo container usa a sintaxe "--volume-from" para referenciar ao primeiro container que foi criado

### ***Backup de um volume***

​	Se esse volume estiver em um container em execução podemos usar o seguinte artificio, criamos um novo container e montamos o volume que esta em execução e fazemos o backup para um outro volume do tipo bind por exemplo. E no final do comando "run" executamos o comando "tar".

```shell
docker container run -ti --mount type=volume,src=dbdados,dst=/data --mout type=bind.src=opt/backup,dst=backup debian tar -cvf /backup/bkp-banco.tar /data
```

