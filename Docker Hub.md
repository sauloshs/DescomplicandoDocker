# ***Docker Hub***

​	Repositório para armazenamento de imagens.

​	A imagem deve receber uma tag com o nome do seu usuário do repositor como no exemplo, que vamos criar uma tag a partir de uma imagem existente.

## ***Padrão de TAG***

```shell
docker image tag <ID da imagem> sauloshs/meu_apache:1.0.0
```

 	Onde "sauloshs" é o meu ID de usuário no repositório. Em seguida devemos fazer login no docker hub.

## ***Login Docker Hub***

```shell
docker login 
```

## ***Docker PUSH***

```shell
docker push sauloshs/meu_apache:1.0.0
```

​	Usado para enviar imagens para o repositório remoto.

