# docker

## Habilitar o Virtual Machine Platform
Execute os seguintes comandos no PowerShell em modo administrador:

dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart

## Instalar o executável do WSL
Baixe o Kernel do WSL 2 neste link: https://docs.microsoft.com/pt-br/windows/wsl/wsl2-kernel e instale o pacote.

## Atribuir a versão default do WSL para a versão 2
A versão 1 do WSL é a padrão no momento, atribua a versão default para a versão 2, assim todas as distribuições Linux instaladas serão já por default da versão 2. Execute o comando com o PowerShell:

wsl --set-default-version 2

## Escolha sua distribuição Linux no Windows Store
Também é possível instalar distribuições Linux pelo Windows Store. Escolha sua distribuição Linux preferida no aplicativo Windows Store, sugerimos o Ubuntu (sem versão) por ser uma distribuição popular e que já vem com várias ferramentas instaladas por padrão.

![image](https://user-images.githubusercontent.com/119737014/229782986-ce302de7-239d-4def-850f-71bdac6a5ebd.png)


## 1 - Instalar o Docker com Docker Engine (Docker Nativo)
A instalação do Docker no WSL 2 é idêntica a instalação do Docker em sua própria distribuição Linux, portanto se você tem o Ubuntu é igual ao Ubuntu, se é Fedora é igual ao Fedora. A documentação de instalação do Docker no Linux por distribuição está aqui, mas vamos ver como instalar no Ubuntu.

Quem está migrando de Docker Desktop para Docker Engine, temos duas opções

Desinstalar o Docker Desktop.
Desativar o Docker Desktop Service nos serviços do Windows. Esta opção permite que você utilize o Docker Desktop, se necessário, para a maioria dos usuários a desinstalação do Docker Desktop é a mais recomendada. Se você escolheu a 2º opção, precisará excluir o arquivo ~/.docker/config.json e realizar a autenticação com Docker novamente através do comando "docker login"
Se necessitar integrar o Docker com outras IDEs que não sejam o VSCode

O VSCode já se integra com o Docker no WSL desta forma através da extensão Remote WSL ou Remote Container.

É necessário habilitar a conexão ao servidor do Docker via TCP. Vamos aos passos:

Crie o arquivo /etc/docker/daemon.json: sudo echo '{"hosts": ["tcp://0.0.0.0:2375", "unix:///var/run/docker.sock"]}' > /etc/docker/daemon.json
Reinicie o Docker: sudo service docker restart
Após este procedimento, vá na sua IDE e para conectar ao Docker escolha a opção TCP Socket e coloque a URL http://IP-DO-WSL:2375. Seu IP do WSL pode ser encontrado com o comando cat /etc/resolv.conf.

Se caso não funcionar, reinicie o WSL com o comando wsl --shutdown e inicie o serviço do Docker novamente.

Instale os pré-requisitos:

sudo apt update && sudo apt upgrade
sudo apt remove docker docker-engine docker.io containerd runc
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
Adicione o repositório do Docker na lista de sources do Ubuntu:

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo \
  "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
Instale o Docker Engine

sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
Dê permissão para rodar o Docker com seu usuário corrente:

sudo usermod -aG docker $USER
Inicie o serviço do Docker:

sudo service docker start
Este comando acima terá que ser executado toda vez que Linux for reiniciado. Se caso o serviço do Docker não estiver executando, mostrará esta mensagem de erro ao rodar comando docker:

Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?
O Docker Compose instalado agora estará na versão 2, para executa-lo em vez de docker-compose use docker compose.
