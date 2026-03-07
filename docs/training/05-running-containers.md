# Executando aplicações no contêiner

## Executando contêineres

Ao executar o comando para iniciar a execução da imagem em um contêiner, o mesmo ficará em execução, então podemos entrar
no _bash_ do contêiner para fazemos o que for necessário, mas ao sair do _bash_, o contêiner é desligado, ficando 
_offline_.

Então, como podemos fazer para que o contêiner fique _offline_ somente quando desejamos isso, somente após 
executarmos o comando para parar o contêiner (`stop`).

Consultando o `docker --help`, vemos a opção `exec`, que realiza o que desejamos. Essa opção permite executar um 
comando em um contêiner em execução.

```shell
$ docker --help
  ...
  exec        Execute a command in a running container
```

Antes de executar o que desejamos, vamos executar o comando `docker run --help`. Vemos a opção `-d`, que serve para executar
o contêiner em _background_, exibindo o identificador do contêiner que foi iniciado.

```shell
$ docker run --help
  ...
  -d, --detach                           Run container in background and print container ID
```

Como o contêiner estará em execução em _background_, quando sairmos do contêiner, o mesmo continuará em execução, 
não sendo fechado e nem deixando _offline_.

Dessa forma, vamos executar o contêiner com a imagem _Ubuntu_ em _background_ (`-d`), com o pseudo terminal para 
podermos trabalhar (`-t`) e em modo interativo no contêiner para que possar ser realizada interações (`-i`). Ao executar
o comando `docker run -dti ubuntu`, conseguiremos manter o contêiner ativo após sairmos dele. Ao executarmos o 
comando, é exibido em tela o identificador completo do contêiner.

```shell
$ docker run -dti ubuntu
e2651ac6e126c13b740cf72b8cba545397ccb084600f2ab5d8cad9df6cc50050
```

Executando o comando `docker ps`, vemos que o contêiner está em execução, exibindo uma parte do identificador.

```shell
$ docker ps
CONTAINER ID   IMAGE     COMMAND       CREATED              STATUS              PORTS     NAMES
e2651ac6e126   ubuntu    "/bin/bash"   About a minute ago   Up About a minute             jovial_lamarr
```

Para realizar alguma operação dentro do contêiner do _Ubuntu_ em execução, como, por exemplo, instalar algum programa, 
vamos utilizar o comando `docker exec -it [id ou nome do contêiner]`, indicamos o que desejamos executar, no 
caso do _bash_, colocamos o `/bin/bash`, dessa forma, o comando completo fica `docker exec -it e2651ac6e126 /bin/bash`

```shell
$ docker exec -it e2651ac6e126 /bin/bash
root@e2651ac6e126:/# ls 
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@e2651ac6e126:/# 
```

Se tentarmos executar o _nano_, vemos que o mesmo não está instalado.

```shell
root@e2651ac6e126:/# nano
bash: nano: command not found
```

Se tentamos instalar o _nano_, não conseguimos, pois as dependências estão desatualizadas.

```shell
root@e2651ac6e126:/# apt -y install nano
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
E: Unable to locate package nano
```

Atualizando as dependências e os repositórios, vamos conseguir instalar o _nano_.

```shell
root@e2651ac6e126:/# apt update
Get:1 http://archive.ubuntu.com/ubuntu noble InRelease [256 kB]
Get:2 http://security.ubuntu.com/ubuntu noble-security InRelease [126 kB]
...
Get:17 http://archive.ubuntu.com/ubuntu noble-backports/universe amd64 Packages [34.6 kB]
Get:18 http://archive.ubuntu.com/ubuntu noble-backports/main amd64 Packages [49.5 kB]
Fetched 36.5 MB in 9s (4131 kB/s)
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
4 packages can be upgraded. Run 'apt list --upgradable' to see them.
```

```shell
root@e2651ac6e126:/# apt upgrade -y
Setting up gcc-14-base:amd64 (14.2.0-4ubuntu2~24.04.1) ...
(Reading database ... 4381 files and directories currently installed.)
Preparing to unpack .../libstdc++6_14.2.0-4ubuntu2~24.04.1_amd64.deb ...
Unpacking libstdc++6:amd64 (14.2.0-4ubuntu2~24.04.1) over (14.2.0-4ubuntu2~24.04) ...
Setting up libstdc++6:amd64 (14.2.0-4ubuntu2~24.04.1) ...
(Reading database ... 4381 files and directories currently installed.)
Preparing to unpack .../libgcc-s1_14.2.0-4ubuntu2~24.04.1_amd64.deb ...
Unpacking libgcc-s1:amd64 (14.2.0-4ubuntu2~24.04.1) over (14.2.0-4ubuntu2~24.04) ...
Setting up libgcc-s1:amd64 (14.2.0-4ubuntu2~24.04.1) ...
(Reading database ... 4381 files and directories currently installed.)
Preparing to unpack .../libgnutls30t64_3.8.3-1.1ubuntu3.5_amd64.deb ...
Unpacking libgnutls30t64:amd64 (3.8.3-1.1ubuntu3.5) over (3.8.3-1.1ubuntu3.4) ...
Setting up libgnutls30t64:amd64 (3.8.3-1.1ubuntu3.5) ...
Processing triggers for libc-bin (2.39-0ubuntu8.7) ...
```

Agora conseguimos instalar o _nano_ com o comando `apt -y install nano`.

Para sair do contêiner, podemos utilizamos o comando `exit`.

Rodando o comando `docker ps`, vemos que o contêiner continua em execução.

```shell
$ docker ps
CONTAINER ID   IMAGE     COMMAND       CREATED          STATUS          PORTS     NAMES        
e2651ac6e126   ubuntu    "/bin/bash"   29 minutes ago   Up 29 minutes             jovial_lamarr
```

Caso acessarmos novamente o contêiner com o comando `exec`, conseguimos entrar novamente no contêiner, e conseguimos 
verificar que o _nano_ continua instalado.

Além do `/bin/bash`, podemos acessar qualquer aplicativo dentro do contêiner, como pode ser visto abaixo. No exemplo 
abaixo, o contêiner executa o comando, exibe a informação e depois é fechado. Mas o contêiner continua em execução, 
sendo exibido no comando `docker ps`.

```shell
$ docker exec -it e2651ac6e126 cat /etc/os-release
PRETTY_NAME="Ubuntu 24.04.4 LTS"
NAME="Ubuntu"
VERSION_ID="24.04"
VERSION="24.04.4 LTS (Noble Numbat)"
VERSION_CODENAME=noble
ID=ubuntu
ID_LIKE=debian
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
UBUNTU_CODENAME=noble
LOGO=ubuntu-logo
```

O contêiner é finalizado, apenas quando executamos o comando `stop`, que vai realizar a finalização do contêiner em 
execução.

```shell
$ docker stop e2651ac6e126
e2651ac6e126
```

Executando o `docker ps`, não temos mais o contêiner em execução.

```shell
$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```
## Excluíndo os contêineres

Ao rodar o comando `docker ps -a` vemos a lista de todos os contêineres que foram executados. Esses contêineres parados
estão utilizando recursos da máquina, inclusive espaço em disco, sendo que o ideal é que como não estamos mais utilizando
o contêiner, podemos excluir eles.

Para excluir o contêiner, vamos utilizar o comando `docker rm [id do contêiner]`.
```shell
$ docker rm e2651ac6e126
e2651ac6e126
```

Ao rodar o comando `docker ps -a` novamente, não vemos mais na listagem de contêineres o com id _e2651ac6e126_.

Outra coisa que pode ocupar bastante espaço na máquina são as imagens, e caso a imagem não for executar mais, não há mais
a necessidade de manter ela no computador.

Executando o comando `docker images` vemos a lista de imagens que foram baixadas.
```shell
$ docker images
IMAGE                ID             DISK USAGE   CONTENT SIZE   EXTRA
hello-world:latest   ef54e839ef54       25.9kB         9.52kB    U
ubuntu:latest        d1e2e92c075e        119MB         31.7MB    U
```

Para excluir a imagem da máquina, podemos utilizar o comando `docker rmi [nome da imagem]`, nesse caso é _rm_ com
o _**i**_ no final, como o nome da imagem que deseja remover. Mas antes de remover a imagem, é necessário remover todos
os contêineres criados dessa imagem.
```shell
$ docker rmi hello-world
Error response from daemon: conflict: unable to delete hello-world:latest (must be forced) - container 5778be475bb2 is using its referenced image ef54e839ef54
```

Vamos remover os contêineres e depois a imagem:

- Listar os contêineres:
  ```shell
  $ docker ps -a
  CONTAINER ID   IMAGE                COMMAND        CREATED       STATUS                    PORTS     NAMES
  bd6ecd50e21b   ubuntu               "/bin/bash"    5 days ago    Exited (0) 5 days ago               determined_ritchie
  e8af02dc386f   ubuntu               "sleep 1500"   6 days ago    Exited (137) 6 days ago             nostalgic_jones
  d878871e3cb8   ubuntu               "sleep 1500"   6 days ago    Exited (137) 6 days ago             relaxed_moore
  55c0f484ce0e   ubuntu               "sleep 10"     6 days ago    Exited (0) 6 days ago               frosty_gauss
  ae72bc2a89a2   ubuntu               "sleep 10"     6 days ago    Exited (0) 6 days ago               competent_proskuriakova
  7821d26e91e1   ubuntu               "sleep 10"     6 days ago    Exited (0) 6 days ago               priceless_elgamal
  f4ebeeb278da   ubuntu               "/bin/bash"    6 days ago    Exited (0) 6 days ago               inspiring_ride
  a264e701766c   hello-world          "/hello"       6 days ago    Exited (0) 6 days ago               pedantic_kirch
  5778be475bb2   hello-world:latest   "/hello"       10 days ago   Exited (0) 10 days ago              teste-123
  ae17d2665a55   hello-world:latest   "/hello"       10 days ago   Exited (0) 10 days ago              unruffled_bose
  ```
- Excluir os contêineres:
  ```shell
  $ docker rm a264e701766c
  a264e701766c
  ---
  $ docker rm ae17d2665a55
  ae17d2665a55
  ---
  $ docker rm 5778be475bb2
  5778be475bb2
  ```
- Listar os contêineres novamente:
  ```shell
  $ docker ps -a
  CONTAINER ID   IMAGE     COMMAND        CREATED      STATUS                    PORTS     NAMES
  bd6ecd50e21b   ubuntu    "/bin/bash"    5 days ago   Exited (0) 5 days ago               determined_ritchie
  e8af02dc386f   ubuntu    "sleep 1500"   6 days ago   Exited (137) 6 days ago             nostalgic_jones
  d878871e3cb8   ubuntu    "sleep 1500"   6 days ago   Exited (137) 6 days ago             relaxed_moore
  55c0f484ce0e   ubuntu    "sleep 10"     6 days ago   Exited (0) 6 days ago               frosty_gauss
  ae72bc2a89a2   ubuntu    "sleep 10"     6 days ago   Exited (0) 6 days ago               competent_proskuriakova
  7821d26e91e1   ubuntu    "sleep 10"     6 days ago   Exited (0) 6 days ago               priceless_elgamal
  f4ebeeb278da   ubuntu    "/bin/bash"    6 days ago   Exited (0) 6 days ago               inspiring_ride
  ```
- Remover a imagem _hello-world_:
  ```shell
  $ docker rmi hello-world
  Untagged: hello-world:latest
  Deleted: sha256:ef54e839ef541993b4e87f25e752f7cf4238fa55f017957c2eb44077083d7a6a
  ```
- Listando as imagens presentes no computador:
  ```shell
  $ docker images
  IMAGE           ID             DISK USAGE   CONTENT SIZE   EXTRA
  ubuntu:latest   d1e2e92c075e        119MB         31.7MB    U
  ```

Quando executamos um contêiner para uma imagem que não está presente no computador, a imagem é baixada antes de ser
executada. Vamos executar o comando `docker run -dti fedora` para baixar e executar a última versão do _Fedora_.
```shell
$ docker run -dti fedora
Unable to find image 'fedora:latest' locally
latest: Pulling from library/fedora
5bc90b3315da: Pull complete
6fc39cdd7940: Download complete
c79496fc4cc5: Download complete
Digest: sha256:781b7642e8bf256e9cf75d2aa58d86f5cc695fd2df113517614e181a5eee9138
Status: Downloaded newer image for fedora:latest
5eafd86f343a669cf8e09ee2d36c28ac2f1b748ec11176d0c15e1bad6952dfdb
```

Listando os contêineres em execução, vemos que a imagem do _Fedora_ está em execução.
```shell
$ docker ps
CONTAINER ID   IMAGE     COMMAND       CREATED          STATUS          PORTS     NAMES
5eafd86f343a   fedora    "/bin/bash"   44 seconds ago   Up 43 seconds             brave_rubin
```

Executando o comando `docker images`, vemos que a imagem do _Fedora_ agora está listada como local.
```shell
$ docker images
IMAGE           ID             DISK USAGE   CONTENT SIZE   EXTRA
fedora:latest   781b7642e8bf        271MB         70.7MB    U
ubuntu:latest   d1e2e92c075e        119MB         31.7MB    U
```

Vamos parar a imagem com o `docker stop`
```shell
$ docker stop 5eafd86f343a
5eafd86f343a
```

## Nomeando os contêineres

Para não precisar ficar utilizando o id dos contêineres em todos os comandos, podemos colocar nomes em nossos contêineres.
Para isso, vamos utilizar o comando `docker run -dti --name [nome customizado do contêiner] [imagem]`
```shell
$ docker run -dti --name Ubuntu-A ubuntu
518879f81b3f29902be0afeef0ac23b4bf43db4c097930055743ec3a1d50f05a
```
```shell
$ docker run -dti --name Fedora-A fedora
29296d9783d13e23f92d9e5045145cda93b664ade33426dd80421419b7edda49
```
```shell
$ docker run -dti --name Ubuntu-B ubuntu
78be9b54a74aa6630b16280fcdfd2e0907de4fd9b46f6211e34328b86f117d08
```
```shell
$ docker ps
CONTAINER ID   IMAGE     COMMAND       CREATED          STATUS          PORTS     NAMES
78be9b54a74a   ubuntu    "/bin/bash"   8 seconds ago    Up 7 seconds              Ubuntu-B
29296d9783d1   fedora    "/bin/bash"   14 seconds ago   Up 13 seconds             Fedora-A
518879f81b3f   ubuntu    "/bin/bash"   37 seconds ago   Up 36 seconds             Ubuntu-A
```
