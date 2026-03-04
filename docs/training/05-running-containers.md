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
