# ***Trabalhando com Repositórios***

## ***Dockerhub***

​	Repositório para armazenamento de imagens.

​	A imagem deve receber uma tag com o nome do seu usuário do repositor como no exemplo, que vamos criar uma tag a partir de uma imagem existente.

### ***Padrão de TAG***

```shell
docker image tag <ID da imagem> sauloshs/meu_apache:1.0.0
```

​	Onde "sauloshs" é o meu ID de usuário no repositório. Em seguida devemos fazer login no docker hub.

### ***Login Dockerhub***

```shell
docker login 
```

### ***Logout Dockerhub***

```shell
docker logout
```

### ***Enviando uma imagem para repositório remoto***

```shell
docker push sauloshs/meu_apache:1.0.0
```

​	Usado para enviar imagens para o repositório remoto.

### ***Executando imagem do repositório remoto***

```shell
docker container run -ti sauloshs/meu_apache:1.0.0
```

## ***Docker Registry***

​	Usado para quem deseja salvar suas imagens em um repositório local. Ele consiste em uma imagem docker que roda na porta 5000.

### ***Instalando o Registry***

```shell
docker container run -d -p 5000:5000 --restart=always --name registry registry:2
```

​	O parâmetro "--restart=always" é usado para que caso aconteça algum problema com o container ele possa ser reiniciado de forma automática.

### ***Registando uma tag para repositório local***

```shell
docker image tag <ID da imagem> localhost:5000/meu_apache:1.0.0
```

​	Nesse exemplo fui utilizado uma imagem existente como referencia.

### ***Enviando uma imagem para repositório local***

```shell
docker image push localhost:5000/meu_apache:1.0.0
```

### ***Executando imagem do repositório local***

```shell
docker container run -d localhost:5000/meu_apache:1.0.0
```

