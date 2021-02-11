# ***Introdução ao Docker***

## ***O que é Container***

​	Container não é uma máquina virtual e sim o isolamento de processos, usuários e recursos do servidor como processamento e memoria dentro de um servidor.

## ***O que é Docker***

​	É uma forma de fazer isolamento, ou seja uma solução de container. 

### ***Bash para instalação do Docker***

```shell
curl -fsSL https://get.docker.com | bash
```

### ***Comandos básicos***

#### ***Listando container***

```shell
docker container ls
docker container ls -a	
```

#### ***Executando container Hello-world***

```shell
docker container run -ti hello-world
```

​	O parâmetro "-ti" é para ter interatividade com o container, quando o container subir já estaremos conectado ao container.

#### ***Executando container***

```shell
docker container run -ti ubuntu	
```

#### ***Saindo de um container***

```shell
crt + p + q	
# Não devemos usar o comando exit e nem crt + d
```

#### ***Entrando em um container em execução***

```shell
docker container attach <ID Container> ou <Nome do container>
```

#### ***Executando container como daemon***

```shell
docker container run -d nginx
```

​	Foi usado o atributo -d para isso.

#### ***Executando comandos em containers sem conectar***

```shell
docker container exec -ti [CONTAINER ID] [COMANDO]
# Exemplo:
docker container exec -ti e4e594922738 bash
```

#### ***Parando e Iniciando container***

```shell
docker container stop <ID container>
docker container start <ID container>
docker container restart <ID container>
docker container pause <ID container> #Pausa o processo do container
docker container unpause <ID container> #Sair do modo pause (Sem o container perder informações).
```

#### ***Inspecionar container***

```shell
docker container inspect <ID container>
```

#### ***Removendo container***

```shell
docker container rm  <ID container>
docker container rm -f <ID container> #Caso o container esteja em execução
```

#### ***Verificando o status do container***

```shell
docker container stats <ID container>
```

#### ***Configurando CPU e Memória para container***

```shell
docker container run -d -m 128M --cpus 0.5 nginx
#Onde -m 128M siguinifica que o meu container pode consumir até 128M do meu host.
#0.5 cpus siguinifica 50% de um core. Se fosse especificado o valor 1 seria o core inteiro. 
docker container update --cpus 0.2 <ID container> #Atualizar informações de configuração de um container em execução
docker container update --cpus 0.4 --memory 64M <ID container>
```

#### ***Listando Imagens***

```shell
docker container image ls
```

