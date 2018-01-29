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

# Observações 

 * Imagens do Docker possuem um sistema de arquivos em camadas (Layered File System) e os benefícios dessa abordagem principalmente para o download de novas imagens.
 * Imagens são Read-Only sempre (apenas para leitura)
 * Containers representam uma instância de uma imagem
 * Como imagens são Read-Only os containers criam nova camada (layer) para guardar as alterações
 * O comando Docker run e as possibilidades de execução de um container
 
 
 ## Caso esteja usando o Docker ToolBox, para criar volumes: 
 
Tente ir até o diretório cursoDocker e digite pwd, deverá aparecer impresso o caminho, parecido com esse /c/Users/xxx/Desktop/cursoDocker

Tente copiar esse caminho e depois rodar o container:

docker run -it -v "/c/Users/xxx/Desktop/cursoDocker:/var/www" ubuntu
 
 
## Rodando código em um container

 1. Acessar o diretório local do projeto
 2. Executar o comando - docker run -p 8080:3000 -d -v "$(pwd):/var/www" -w "/var/www" node npm start
 
## Criando um dockerfile

``` js
FROM node:latest  //Base
MAINTAINER Ariel S Azzi  //criador
ENV NODE_END=production  //variavel de ambiente
ENV PORT=3000   //variavel de ambiente 
COPY . /var/www //o que vai dentro da imagem 
WORKDIR /var/www // diretorio
RUN npm install //comando a ser executado
ENTRYPOINT ["npm", "start"] // executa quando o container for carregado
EXPOSE $PORT //porta que a aplicação vai rodar
``` 

### Criando a imagem 
 * docker build -f Dockerfile -t arielazzi/node .
 
 ### Criando um container a partir da nossa imagem
 * docker run -d -p 8080:3000 arielazzi/node
 
 
 ## Subindo a imagem do Docker Hub
 
 1. docker login
 2. docker push arielazzi/node
 
*para utiliza um container do dockerhub basta executar o comando `docker pull arielazzi/node`
 

 ## Comunicação entre containers utilizando os seus nomes

- Criando um rede 
```
docker network create --driver bridge minha-rede
```

- Atrelando um container a rede criada
```
docker run -it --name meu-container-de-ubuntu --network minha-rede ubuntu
```


 ## Lista de comandos
 
| Comando                            | Descrição                                                                      | 
|------------------------------------| ------------------------------------------------------------------------------ | 
| docker version                     | exibe a versão do docker                                                       |
| docker run NOME_DA_IMAGEM          | cria um container com a respectiva imagem passada como parâmetro               |
| docker ps                          | exibe quais os containers que estão sendo executados no momento                |
| docker ps -a                       | exibe todos os containers                                                      |
| docker run -it NOME_DA_IMAGEM      | integra o terminal de dentro do container (para trabalhar dentro do container) |
| docker start ID_DO_CONTAINER       | executa um container já criado                                                 |
| docker stop ID_DO_CONTAINER        | parar um container                                                             |
| docker start -a -i ID_DO_CONTAINER | executa um container já criado / `-a (attach)`: integra os terminais / `-i (interactive)`: acessa o terminal|
| docker rm ID_DO_CONTAINER          | remove um container                                                            |
| docker container prune             | remove todos os containers parados                                             |
| docker rmi NOME_DA_IMAGEM          | remove uma imagem                                                              |
| docker run -d NOME_DA_IMAGEM       | executa sem atrelar o nosso terminal ao terminal do container                  |
| docker run -d -P NOME_DA_IMAGEM    | atribuir uma porta aleatória do mundo externo a porta interna do container     |
| docker port ID_DO_CONTAINER        | exibe as portas do container                                                   |
| docker-machine ip                  | ip da máquina virtual (caso esteja utilizando Docker ToolBox)                  |
| docker run -d -P --name NOME NOME_DA_IMAGEM | Atribui um nome para o container (assim podemos rodas docker stop `NOME`) |
| docker run -d -p NUMERO_DA_PORTA NOME_DA_IMAGEM | Define uma porta manualmente (docker run -d -p 12345:80 dockersamples/static-site) |
| docker run -d -P -e NOME_DA_VARIAVEL_DE_AMBIENTE NOME_DA_IMAGEM | Atribuindo uma variável de ambiente (docker run -d -P -e AUTHOR="Douglas Q" dockersamples/static-site) |
| docker stop $(docker ps -q)        | Parando todos os containers de uma só vez                                      |
| docker stop -t 0 $(docker ps -q)   | Parando todos os containers de uma só vez sem aguardar os 10s                  |
| docker run -v "/var/www" NOME_DA_IMAGEM   | cria um volume               |
| docker inspect ID_DO_CONTAINER     | Inspeciona o container             |
| docker run -it -v "CAMINHO:/var/www" NOME_DA_IMAGEM   | criando um volume              |
|docker-compose up                   | sobe os serviços criados|
|docker-compose down                 | para os serviços criados|
|docker-compose ps                   | lista os serviços que estão rodando|
|docker exec -it alura-books-1 ping node2 | executa o comando ping node2 dentro do container alura-books-1|





### Para saber mais

DevOps é um tópico bem amplo que envolve tanto aspectos culturais como técnicos, mas possui como principal objetivo aumentar a qualidade e eficiência da entrega de software. DevOps é uma metodologia que visa integrar os times de desenvolvimento com infraestrutura e o Docker está tendo um papel importante nessa tarefa.

Repare que com Docker os desenvolvedores não precisam se preocupar em configurar um ambiente de desenvolvimento específico de cada vez. Em vez disso, eles podem se concentrar na construção de um código de boa qualidade. Isso, obviamente, leva à aceleração nos esforços de desenvolvimento. O Docker facilita muito construtir o ambiente: é rapido, simples e confiável.

No outro lado, para a equipe de operações de TI / Sysadmins, o Docker possibilita configurar ambientes que são exatamente como um servidor de produção e permite que qualquer pessoa trabalhe no mesmo projeto com exatamente as mesmas configurações, independentemente do ambiente de host local. As configurações são descritas em arquivos simples facilmente aplicáveis pelo desenvolvedor.

Com a padronização de um entregável Docker é possível que o desenvolvedor tenha um ambiente similar ao de produção na sua máquina sem todo o custo de configuração e o sysadmin consiga lidar apenas com um tipo de entregável conseguindo, desta forma, dar atenção aos desafios de monitoramento e orquestração para que nada dê errado. Neste caso o melhor para os dois.


## Principais comandos 

### Segue a lista com os principais comandos utilizados durante o curso:

- Comandos relacionados à informações
  - docker version - exibe a versão do docker que está instalada.
  - docker inspect ID_CONTAINER - retorna diversas informações sobre o container.
  - docker ps - exibe todos os containers em execução no momento.
  - docker ps -a - exibe todos os containers, independente de estarem em execução ou não.

- Comandos relacionados à execução
  - docker run NOME_DA_IMAGEM - cria um container com a respectiva imagem passada como parâmetro.
  - docker run -it NOME_DA_IMAGEM - conecta o terminal que estamos utilizando com o do container.
  - docker run -d -P --name NOME dockersamples/static-site - ao executar, dá um nome ao container.
  - docker run -d -p 12345:80 dockersamples/static-site - define uma porta específica para ser atribuída à porta 80 do container, neste caso 12345.
  - docker run -v "CAMINHO_VOLUME" NOME_DA_IMAGEM - cria um volume no respectivo caminho do container.
  - docker run -it --name NOME_CONTAINER --network NOME_DA_REDE NOME_IMAGEM - cria um container especificando seu nome e qual rede deverá ser usada.

- Comandos relacionados à inicialização/interrupção
  - docker start ID_CONTAINER - inicia o container com id em questão.
  - docker start -a -i ID_CONTAINER - inicia o container com id em questão e integra os terminais, além de permitir interação entre ambos.
  - docker stop ID_CONTAINER - interrompe o container com id em questão.
 
- Comandos relacionados à remoção
  - docker rm ID_CONTAINER - remove o container com id em questão.
  - docker container prune - remove todos os containers que estão parados.
  - docker rmi NOME_DA_IMAGEM - remove a imagem passada como parâmetro.

- Comandos relacionados à construção de Dockerfile
  - docker build -f Dockerfile - cria uma imagem a partir de um Dockerfile.
  - docker build -f Dockerfile -t NOME_USUARIO/NOME_IMAGEM - constrói e nomeia uma imagem não-oficial.
  - docker build -f Dockerfile -t NOME_USUARIO/NOME_IMAGEM CAMINHO_DOCKERFILE - constrói e nomeia uma imagem não-oficial informando o caminho para o Dockerfile.

- Comandos relacionados ao Docker Hub
  - docker login - inicia o processo de login no Docker Hub.
  - docker push NOME_USUARIO/NOME_IMAGEM - envia a imagem criada para o Docker Hub.
  - docker pull NOME_USUARIO/NOME_IMAGEM - baixa a imagem desejada do Docker Hub.

- Comandos relacionados à rede
  - hostname -i - mostra o ip atribuído ao container pelo docker (funciona apenas dentro do container).
  - docker network create --driver bridge NOME_DA_REDE - cria uma rede especificando o driver desejado.
