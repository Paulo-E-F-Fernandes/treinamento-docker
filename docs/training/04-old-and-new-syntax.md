# A VELHA sintaxe e a NOVA sintaxe

Existem duas maneiras de utilizarmos o _Docker_, sendo que podemos utilizar os comandos da sintaxe VELHA e também utilizando
os comandos da sintaxe NOVA.

A sintaxe antiga é que estamos utilizando atualmente:
- `docker ps`, `docker ps -a`, `docker run`, ...

Para a nova sintaxe, utilizamos o comando com o _container_, por exemplo:
- `docker container ls`, `docker container ls -a`, `docker container run`, ...

A sintaxe antiga é mais simplificada, enquanto a sintaxe nova é necessário colocar a palavra _container_ no comando.

Em caso de dúvida, podemos utilizar o comando `--help`. Então utilizando o comando `docker --help`, temos diretamente as 
opções utilizadas com o comando _Docker_. Ao utilizar o comando `docker container --help`, temos a lista das opções que 
podem ser utilizadas com o comando `docker container`.