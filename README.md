# Docker

## Container

Um container funcionará junto do nossos sistema operacional base, e conterá a nossa aplicação, ou seja, a aplicação será executada dentro dele. Criamos um container para cada aplicação, e esses containers vão dividir as funcionalidades do sistema operacional:

![Container](https://s3.amazonaws.com/caelum-online-public/646-docker/01/imagens/container.png "Container")

Não temos mais um sistema operacional para cada aplicação, já que agora as aplicações estão dividindo o mesmo sistema operacional, que está em cima do nosso hardware. Os próprios containers terão a lógica que se encarregará dessa divisão.

Assim, com somente um sistema operacional, reduzimos os custos de manutenção e de infraestrutura como um todo.

## As tecnologias do Docker
O Docker nada mais é do que uma coleção de tecnologias para facilitar o deploy e a execução das nossas aplicações. A sua principal tecnologia é a Docker Engine, a plataforma que segura os containers, fazendo o intermédio entre com o sistema operacional.

Outras tecnologias do Docker:
 * `Docker Compose` - um jeito fácil de definir e orquestrar múltiplos containers;
 * `Docker Swarm` - uma ferramenta para colocar múltiplos docker engines para trabalharem juntos em um cluster;
 * `Docker Hub` -  um repositório com mais de 250 mil imagens diferentes para os nossos containers; 
 * `Docker Machine` - uma ferramenta que nos permite gerenciar o Docker em um host virtual;


## Instalação no Windows

Existem duas possibilidades para instalar o Docker no Windows. Temos a principal, utilizando o [Docker for Windows](https://store.docker.com/editions/community/docker-ce-desktop-windows).

Requisitos do uso do Docker for Windows, ou seja, devemos possuir um Windows com:

 * Arquitetura 64 bits.
 * Versão Pro, Enterprise ou Education.
 * Virtualização habilitada.
 
 ### Instalação alternativa no Windows
 
 É uma forma alterantiva caso não contemple os requisitos da versão `for windows`. Ainda assim, precisamos garantir que o nosso Windows seja 64bits e que ele tenha a virtualização habilitada. [Link para download](https://download.docker.com/win/stable/DockerToolbox.exe)
 
 
 ### A diferença entre imagens e containers

A imagem é como se fosse uma receita de bolo, uma série de instruções que o Docker seguirá para criar um container, que irá conter as instruções da imagem, do hello-world. Criado o container, o Docker executa-o. Então, tudo isso é feito quando executamos o docker run hello-world.
 
 ## Lista de comandos
 
| Comando                              | Descrição                                                                      | 
|:------------------------------------:| ------------------------------------------------------------------------------ | 
| `docker version`                     | exibe a versão do docker                                                       |
| `docker run NOME_DA_IMAGEM`          | cria um container com a respectiva imagem passada como parâmetro               |
| `docker ps`                          | exibe quais os containers que estão sendo executados no momento                |
| `docker ps -a`                       | exibe todos os containers                                                      |
| `docker run -it NOME_DA_IMAGEM`      | integra o terminal de dentro do container (para trabalhar dentro do container) |
| `docker start ID_DO_CONTAINER`       | executa um container já criado                                                 |
| `docker stop ID_DO_CONTAINER`        | parar um container                                                             |
| `docker start -a -i ID_DO_CONTAINER` | executa um container já criado / `-a (attach)`: integra os terminais / `-i (interactive)`: acessa o terminal|
| `docker rm ID_DO_CONTAINER`          | remove um container                                                            |
| `docker container prune`             | remove todos os containers parados                                             |
| `docker rmi NOME_DA_IMAGEM`          | remove uma imagem                                                              |
| `docker run -d NOME_DA_IMAGEM`       | executa sem atrelar o nosso terminal ao terminal do container                  |
| `docker run -d -P NOME_DA_IMAGEM`    | atribuir uma porta aleatória do mundo externo a porta interna do container     |
| `docker port ID_DO_CONTAINER`        | exibe as portas do container                                                   |
| `docker-machine ip`                  | ip da máquina virtual (caso esteja utilizando Docker ToolBox)                  |
| `docker run -d -P --name NOME NOME_DA_IMAGEM` | Atribui um nome para o container (assim podemos rodas docker stop `NOME`) |
| `docker run -d -p NUMERO_DA_PORTA NOME_DA_IMAGEM` | Define uma porta manualmente (docker run -d -p 12345:80 dockersamples/static-site) |
| `docker run -d -P -e NOME_DA_VARIAVEL_DE_AMBIENTE NOME_DA_IMAGEM` | Atribuindo uma variável de ambiente (docker run -d -P -e AUTHOR="Douglas Q" dockersamples/static-site) |
| `docker stop $(docker ps -q)`        | Parando todos os containers de uma só vez                                      |
| `docker stop -t 0 $(docker ps -q)`   | Parando todos os containers de uma só vez sem aguardar os 10s                  |
