# Comandos utilizados no _console_

## Comandos git

* Criado o repositório _git_ vazio para o projeto
  * `git init`
* Após criar um repositório remoto no [GitHub](https://github.com/)
  * Como o repositório local já existia, utilizei os comandos abaixo
	```shell
	git remote add origin git@github.com:Paulo-E-F-Fernandes/treinamento-docker.git
	git branch -M main
	git push -u origin main
	```
  * Para criar um novo repositório local e associar ao repositório remoto
	```shell
	echo "# treinamento-docker" >> README.md
	git init
	git add README.md
	git commit -m -S "first commit"
	git branch -M main
	git remote add origin git@github.com:Paulo-E-F-Fernandes/treinamento-docker.git
	git push -u origin main
	```
    * O _**-S**_ no comando do _commit_ é para realizar o _commit_ assinado (signing), sendo que isso só funciona 
	  quando o repositório remoto está com a chave de verificação cadastrada.
    * Também é possível configurar o _Intellij_ para realizar o _commit_ assinado em:
      * _**Settings**_ (or _**Preferences**_ on macOS) >> _**Version Control**_ >> _**Git**_ > clicar no botão 
		_**Configure GPG Key...**_
      * Na tela de diálogo que é aberta, marcar para _**Sign commits with GPG key**_ e selecionar a chave desejada.
