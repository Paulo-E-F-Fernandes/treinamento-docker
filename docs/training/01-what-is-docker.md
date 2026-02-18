# O que é o DOCKER

O termo _Docker_ se refere a um projeto da comunidade _open source_, as ferramentas resultantes desse projeto e 
também a empresa _Docker_, a principal apoiadora do projeto.

O fato da tecnologia e da empresa ter o mesmo nome, pode gerar confusão.

Então, para o melhor entendimento:
* O _software_ de TI _"Docker"_ é a tecnologia de conteinerização para a criação e uso de contêineres _Linux_;
* A comunidade _open source_ do _"Docker"_ trabalha para melhore essa tecnologia para todos os usuários;
* A empresa _"Docker"_ se baseia no trabalho da comunidade, tornando o mesmo mais seguro, e compartilha os avanços 
  com a comunidade em geral:
  * Oferece aos clientes corporativos o suporte necessário para as tecnologias aprimoradas.

Com o _Docker_, podemos lidar com os contêineres como se fossem **máquinas virtuais modulares** e extremamente leves.

Os contêineres oferecem uma maior flexibilidade para criar, implantar, copiar e migrar de um ambiente para outro, 
otimizando as aplicações em nuvem privada e púbicas (AWS, Google Cloud, ...).

## O que são os contêineres

Um contêiner é um conjunto de um ou mais processos organizados isoladamente do sistema operacional.

Todos os arquivos necessários para executar são fornecidos por uma **imagem** distinta.

Na prática, os contêineres são portáteis e consistentes durante toda a migração entre os ambientes de desenvolvimento,
teste, homologação e produção.

Essas características tornam os contêineres numa opção muito mais rápida do que os ambientes baseados em máquinas 
virtuais tradicionais.

## Como funcionam os contêineres

Durante o desenvolvimento de uma aplicação, o desenvolvedor está trabalhando numa máquina específica, com o 
ambiente de desenvolvimento com uma configuração específica. Outros desenvolvedores podem ter configurações 
diferentes, ao mesmo tempo que os ambientes de teste e produção podem ter padronizações, configurações e arquivos 
auxiliares próprios.

A aplicação desenvolvida é baseada em configurações, bibliotecas, dependências e arquivos específicos, como uma 
versão específica do _MySQL_, por exemplo. Para padronizar corretamente as versões utilizadas, com os 
**contaniners**, podemos pegar o ambiente que está na empresa, nos ambientes de teste ou produção, e replicar no 
ambiente de desenvolvimento de maneira rápida e simplificada, sem a necessidade de criar máquinas virtuais, instalar o 
sistema operacional e nem instalar a versão do _MySQL_.

Com os contêineres, podemos "emular" o ambiente da empresa no próprio ambiente de desenvolvimento sem a 
necessidade de recriar o ambiente presente no servidor, facilitando o funcionamento em diferentes ambientes, e
garantindo a qualidade e implementação sem esforço e necessidades de reescrever ou altera o código-fonte.

## Diferença entre Virtualização e contêineres

As duas tecnologias são distintas, porém complementares:

* Com a virtualização, é possível executar sistemas operacionais simultaneamente em um único sistema de _hardware_:
  * Em uma máquina _Windows_, podemos criar uma máquina virtual e executar outro sistema operacional, como o _Linux_ ou 
    até _Windows 7_.
* Os contêineres compartilham o mesmo _**kernel**_ do sistema operacional e isolam os processos da aplicação do 
  restante do sistema operacional;
* Na imagem abaixo temos uma comparação do uso de máquinas virtualizadas com contêineres _Docker_:
  * Quando utilizamos a virtualização, para cada ambiente criado, além do _hardware_ e do sistema operacional 
    hospedeiro, necessitamos temos uma _hypervisor_, e dentro dela, colocamos o sistema operacional convidado e 
    instalamos todos os programas necessários, além de alocar a quantidade de memória RAM necessária para o 
    funcionamento de todo o ambiente como, por exemplo, 5GB de RAM:
    * Para replicar esse ambiente, é necessário mais 5GB de RAM, sem contar com a quantidade de uso de disco;
    * Esse processo de replicação pode ser oneroso e demorar um tempo grande de replicação;
    * Além disso, é preciso ter uma quantidade grande de _hardware_, como disco e memória RAM para conseguir 
      realizar a replicação dos ambientes.
  * Quando utilizamos o contêiner _Docker_, temos uma arquitetura mais modular e ocupando menos espaço em disco:
    * Precisamos do _hardware_ e do sistema operacional hospedeiro e colocar o _Docker Engine_:
      * Vamos colocar nossos contêineres isolados, podendo especificar para cada um deles, a quantidade de mémoria 
        necessária que pode ser utilizado para cada contêiner, tendo um controle melhor do uso do _hardware_;
      * Ao baixar uma imagem, e replicar novos ambientes, a imagem não precisar se baixada novamente, pois será 
        utilizada a mesma imagem baixada anteriormente, então caso as imagens do _Python_, _PHP_ e _MySQL_ ocupem 
        200MB ao total, ao replicar em novo ambiente, não teremos 400MB e sim apenas 200MB, pois a mesma imagem é 
        utilizada.
      * Para o ambiente de contêineres, conseguimos ocupar menos espaço em disco.
  * Com o uso do contêiner, se for necessário replicar o _MySQL_ 10 vezes, podemos fazer isso sem a necessidade de 
    replicar o _PHP_ e o _Python_;
    ![Virtualização x contêineres](/assets/images/01-comparacao-virtulizacao-containers.png)
  * Dentre outras vantagens do uso de contêineres em relação a uso da virtualização, a principal delas, é o tempo de 
    implementação.

## Função dos contêineres para o mercado de TI

* Os contêineres tem contribuído para o desmembramento de aplicações monolíticas, ou seja, que são gigantes, em 
  microsserviços.
* Microsserviços são arquiteturas de _software_, que consiste em construir aplicações em serviços independentes, 
  sendo que a comunicação entre si é feita utilizando APIs.
* Um exemplo que podemos citar, é o de login do usuário, o qual pode estar consumindo muitos recursos e gerando 
  lentidão:
  * Então podemos extrair o serviço de login de usuário para um microsserviço, extraindo esse serviço da aplicação 
    como um todos, não sendo necessário passar todas a aplicação monolítica para microsserviço;
  * Esse microsserviço de login pode ser replicados para dois ou mais contêineres, dessa forma podemos atender 
    melhor o sistema de login sem a necessidade de replicar o restante do sistema.

## Vantagens de utilizar microsserviços

* Quando quebramos uma aplicação monolítica e grande em várias pequenas, conseguimos escalar elas de maneira separada;
* Caso o serviço de autenticação seja chamada várias vezes durante a sessão de um usuário, a carga nesse serviço 
  será bem maior que em outros serviços;
* Com microsserviços, podemos escalar apenas uma parte do sistema, sem ter a necessidade de escalar toda a aplicação,
  algo que ocorre em uma arquitetura monolítica;
* Os microsserviços podem ser escritos em outra linguagem de programação. 