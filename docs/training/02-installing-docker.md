# Instalando o DOCKER

Para mais informações sobre a _Docker_ e a sua instalação, podemos acessar o site oficial [Docker](https://docs.docker.com/).

Para realizarmos a instalação no _Linux_, devemos acessar a sessão de instalação da _Docker Engine_, que pode ser acessada 
pela _URL_ https://docs.docker.com/engine/install/.

* Nessa sessão temos as orientações para a instalação nas diferentes distribuições _Linux_;
* Para realizar a instalação, é necessário acessar a sessão da distribuição _Linux_ que está sendo utilizada e seguir os 
passos descritos.
 
Para a instalação no _Windows_ ou _Mac_, devemos utilizar o _Docker Desktop_, sendo que pode ser acessado pela _URL_ 
https://docs.docker.com/desktop/.

* Por padrão, a instalação do _Docker Desktop_ é realizada em `C:\Program Files\Docker\Docker`.

Após a realização da instalação, para verificar se a mesma foi concluída com sucesso

* Acessar o terminal e digitar o comando `docker version`;
* Se tivermos retorno do comando acima, significa que o _Docker_ está instalado e rodando corretamente.

## Comandos útils no terminal

* Para verificar a versão instalada
	```shell
	docker --version
	Docker version 29.2.1, build a5c7197
	```

* Para verificar a versão do _docker compose_
	```shell
	docker-compose --version
	Docker Compose version v5.0.2
	```

* Para listar todos os contêineres em execução
	```shell
	docker ps
	CONTAINER ID    IMAGE      COMMAND                   CREATED          STATUS          PORTS    NAMES
	81013d6b73ae    myimage    "cmd.exe /S /C ping …"    5 minutes ago    Up 5 minutes             musing_archimedes
	```
  * Onde:
    * **CONTAINER ID:** O identificador único do contêiner
    * **IMAGE:** A imagem a partir da qual o contêiner foi construído
    * **COMMAND:** O comando executado quando o contêiner foi iniciado
    * **CREATED:** Há quanto tempo o contêiner foi iniciado
    * **STATUS:** A situação atual (ex.: _"Up X minutes"_, _"Exited"_).
    * **PORTS:** Mapeamentos de portas expostas 
    * **NAMES:** Um nome para o contêiner, gerado aleatoriamente ou definido pelo usuário
  * Outros exemplos de código:
    * `docker ps -a` Listar todos os contêineres, incluindo aqueles que foram finalizados ou parados
    * `docker ps -n 1` Lista o último contêiner criado
    * `docker ps -q` Lista somente os IDs dos contêineres
    * `docker ps --filter "ancestor=nginx:alpine"` Lista contêineres que foram criados com uma imagem específica (ex.: nginx:alpine).
