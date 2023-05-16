# Docker Desktop

## link da imagem
https://hub.docker.com/_/microsoft-mssql-server

## Baixar a imagem do sql server
``` bash
docker pull mcr.microsoft.com/mssql/server:2022-latest
```

## Rodar o container

``` bash
docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=yourStrong(!)Password" -p 1433:1433 -d mcr.microsoft.com/mssql/server:2022-latest
```

## DockerFile

``` bash
FROM mcr.microsoft.com/mssql/server:2022-latest
ENV SA_PASSWORD=Teste01&
ENV ACCEPT_EULA=Y     

RUN mkdir -p /usr/work
COPY *.sql /usr/work/

WORKDIR /usr/work

RUN ( /opt/mssql/bin/sqlservr & ) \
    | grep -q "Service Broker manager has started" \
    && /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P $SA_PASSWORD -i script.sql \
    && pkill sqlservr
```

# docker WSL 2

## Habilitar o Virtual Machine Platform
Execute os seguintes comandos no PowerShell em modo administrador:

``` bash
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

## Instalar o executável do WSL
Baixe o Kernel do WSL 2 neste link: https://docs.microsoft.com/pt-br/windows/wsl/wsl2-kernel e instale o pacote.

## Atribuir a versão default do WSL para a versão 2
A versão 1 do WSL é a padrão no momento, atribua a versão default para a versão 2, assim todas as distribuições Linux instaladas serão já por default da versão 2. Execute o comando com o PowerShell:

``` bash
wsl --set-default-version 2
```

## Escolha sua distribuição Linux no Windows Store
Também é possível instalar distribuições Linux pelo Windows Store. Escolha sua distribuição Linux preferida no aplicativo Windows Store, sugerimos o Ubuntu (sem versão) por ser uma distribuição popular e que já vem com várias ferramentas instaladas por padrão.

![image](https://user-images.githubusercontent.com/119737014/229782986-ce302de7-239d-4def-850f-71bdac6a5ebd.png)


## 1 - Instalar o Docker com Docker Engine (Docker Nativo)
Instale os pré-requisitos:

``` bash
sudo apt update && sudo apt upgrade
sudo apt remove docker docker-engine docker.io containerd runc
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
```

### Adicione o repositório do Docker na lista de sources do Ubuntu:

``` bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo \
  "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

### Instale o Docker Engine

``` bash
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

### Dê permissão para rodar o Docker com seu usuário corrente:

``` bash
sudo usermod -aG docker $USER
```

### Inicie o serviço do Docker:

``` bash
sudo service docker start
```

Este comando acima terá que ser executado toda vez que Linux for reiniciado. Se caso o serviço do Docker não estiver executando, mostrará esta mensagem de erro ao rodar comando docker:

Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?
O Docker Compose instalado agora estará na versão 2, para executa-lo em vez de docker-compose use docker compose.

### Erro ao iniciar o Docker no Ubuntu 22.04

> Se mesmo ao iniciar o serviço do Docker acontecer o seguinte erro ou similar:
>
> `Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?`
> Rode o comando `sudo update-alternatives --config iptables` e escolha a opção 1 `iptables-legacy`
>
> Rode novamente o `sudo service docker start`. Rode algum comando Docker como `docker ps` para verificar se está funcionando corretamente. Se não mostrar o erro acima, está ok.

### Cofiguração do terminal
Baixar o arquivo de configuração do terminal e utilizar em seu local

# Redis

https://drive.google.com/file/d/1EBfnADQTMLLUOOg5w4wXYf1WyPwPnpOB/view?usp=sharing
