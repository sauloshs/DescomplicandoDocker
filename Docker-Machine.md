# ***Docker Machine***

## ***Instalação***

### ***Instalando no Linux***

```sh
curl -L https://github.com/docker/machine/releases/download/v0.16.1
/docker-machine-`uname -s`-`uname -m` >/tmp/docker-machine
chmod +x /tmp/docker-machine 
sudo cp /tmp/docker-machine /usr/local/bin/docker-machine
```

### ***Instalando no macOS***

```shell
curl -L https://github.com/docker/machine/releases/download/v0.16.1
/docker-machine-`uname -s`-`uname -m` >/usr/local/bin/docker-machine 
chmod +x /usr/local/bin/docker-machine
```

### ***Instalando no Windows (Necessário Git bash)***

```sh
if [[ ! -d "$HOME/bin" ]]; then mkdir -p "$HOME/bin"; fi
curl -L https://github.com/docker/machine/releases/download/v0.16.1
/docker-machine-Windows-x86_64.exe > "$HOME/bin/docker-machine.exe"
chmod +x "$HOME/bin/docker-machine.exe"
```

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

