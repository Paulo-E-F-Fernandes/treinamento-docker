# Realizando o _download_ da primeira imagem

Vamos realizar o _download_ da primeira imagem para a criamos o primeiro contêiner e executar o mesmo.

## Baixando pelo site e executando pelo terminal

Vamos realizar o _download_ pelo site _Docker Hub_, que pode ser acessado pela _URL_ https://hub.docker.com/:

* Para utilizar o _Docker Hub_, precisamos criar um usuário e depois devemos selecionar o plano desejado, sendo que 
  inicialmente, o plano _free_ já é o suficiente;
* Caso já tenha criado um usuário para utilizar no _Docker_, podemos utilizar o mesmo usuário para acessar o _Docker 
  Hub_.

A primeira imagem que vamos procurar e fazer o _download_ é a `hello-world`:

* Ao selecionar a imagem que desejamos, será aberta a página com as informações da mesma:
  * O código-fonte da imagem `hello-world` pode ser encontrado em https://github.com/docker-library/hello-world
* Na página da imagem, podemos ver as opções para realizar o _download_:
  * Temos a opção para executar a imagem no _Docker Desktop_;
  * Mas ao clicar na opção "⋮", vemos o _Docker pull command_, que pode ser utilizado no terminal;
  * ![Download Docker Hub](/assets/images/02-docker-hub-dowload.png)
* Ao executar o comando `docker pull hello-world` no terminal, temos o retorno abaixo:
  * ```shell
    Using default tag: latest
    latest: Pulling from library/hello-world
    17eec7bbc9d7: Pull complete
    ea52d2000f90: Download complete
    Digest: sha256:ef54e839ef541993b4e87f25e752f7cf4238fa55f017957c2eb44077083d7a6a
    Status: Downloaded newer image for hello-world:latest
    docker.io/library/hello-world:latest
    ```
  * Como não especificamos nenhuma _tag_ ou versão, será realizado o _download_ da última versão disponível, e isso 
    pode ser visto no retorno do comando no terminal, no trecho `Using default tag: latest`.
* Para verificar se a imagem foi baixada corretamente, podemos executar o comando `docker images`:
  * O retorno do comando acima é o seguinte:
    ```shell
    IMAGE                ID             DISK USAGE   CONTENT SIZE   EXTRA
    hello-world:latest   ef54e839ef54       25.9kB         9.52kB
    ```
  * Para executar a imagem baixada, podemos executar o comando `docker run hello-world`, sendo que o retorno do 
    comando pode ser visto abaixo: 
  * ```shell
    Hello from Docker!
    This message shows that your installation appears to be working correctly.
  
    To generate this message, Docker took the following steps:
    1. The Docker client contacted the Docker daemon.
       2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
          (amd64)
       3. The Docker daemon created a new container from that image which runs the
          executable that produces the output you are currently reading.
       4. The Docker daemon streamed that output to the Docker client, which sent it
          to your terminal.
  
    To try something more ambitious, you can run an Ubuntu container with:
    $ docker run -it ubuntu bash
  
    Share images, automate workflows, and more with a free Docker ID:
    https://hub.docker.com/
  
    For more examples and ideas, visit:
    https://docs.docker.com/get-started/
    ``` 
* Para verificar as imagens em execução, podemos utilizar o comando `docker ps`, mas como essa imagem apenas inicia, 
  executa o seu conteúdo e depois é finaliza, não será exibido nada no retorno do _docker ps_;
* Dessa forma, precisamos executar o comando `docker ps -a`, para conseguirmos visualizar as imagens executadas 
  recentemente:
  ```shell
  CONTAINER ID   IMAGE         COMMAND    CREATED         STATUS                     PORTS     NAMES
  490d7989b72a   hello-world   "/hello"   6 minutes ago   Exited (0) 6 minutes ago             wizardly_joliot
  ```
* Após realizar o _download_ e a execução da imagem pelo terminal, podemos ver as informações também nas sessões 
  _**Images**_ e _**Containers**_ do _Docker Desktop_:
  * Ao baixar a imagem pelo terminal (`docker pull hello-world`), a imagem baixada pode ser visualizada na sessão 
    _**Images**_ do _Docker Desktop_;
  * Ao executar a imagem pelo terminal (`docker run hello-world`), a execução pode ser visualizada na sessão 
    _**Containers**_ do _Docker Desktop_.

## Baixando e executando pelo Docker Desktop

No site do _Docker Hub_, ao utilizarmos a opção _Run in Docker Desktop_ da página da imagem _Docker_, é solicitada a
permissão para o navegador abrir a página da imagem no _Docker Desktop_ e se for autorizada, a página da imagem 
será exibida no _Docker Desktop_.

Na página da imagem aberta no _Docker Desktop_, temos as opções para instalar (_Pull_) ou executar (_Run_) a imagem 
selecionada.

![Exemplo imagem no Docker Desktop](/assets/images/03-imagem-no-docker-desktop.png)

No _Docker Desktop_, também possui a sessão _Docker Hub_, a qual podemos procurar pelas imagens e posteriormente 
realizar o _download_ das mesmas.