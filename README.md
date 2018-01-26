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


## Instalação principal no Windows
Existem duas possibilidades para instalar o Docker no Windows. Temos a principal, utilizando o [Docker for Windows](https://store.docker.com/editions/community/docker-ce-desktop-windows).

Requisitos do uso do Docker for Windows, ou seja, devemos possuir um Windows com:

 * Arquitetura 64 bits.
 * Versão Pro, Enterprise ou Education.
 * Virtualização habilitada.
 
 ### Instalação alternativa no Windows
 
 É uma forma alterantiva caso não contemple os requisitos da versão `for windows`. Ainda assim, precisamos garantir que o nosso Windows seja 64bits e que ele tenha a virtualização habilitada. [Link para download](https://download.docker.com/win/stable/DockerToolbox.exe)
 
 
 ## Lista de comandos
 
| Comando                     | Descrição                                                        | 
|:---------------------------:|:----------------------------------------------------------------:| 
| `docker version`            | exibe a versão do docker                                         |
| `docker run NOME_DA_IMAGEM` | cria um container com a respectiva imagem passada como parâmetro |
 
 
