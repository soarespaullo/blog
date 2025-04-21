---
title: Guia Completo para Criação de Chaves SSH (no Linux e Windows)
description: Guia Completo para Criação de Chaves SSH (no Linux e Windows)
author: soarespaullo
date: 2025-04-18 14:50:00
categories: [Linux, SysAdmin]
tags: [Tutoriais, Servidor, SysAdmin, SSH]
math: true
mermaid: true
image:
  path: /assets/img/img-blog/ssh-key/ssh-keey.png
  alt: Guia Completo para Criação de Chaves SSH (no Linux e Windows)

---

# Gerando chaves SSH no GNU/Linux

O primeiro passo para configurar a autenticação de chaves SSH para seu servidor é gerar um par de chaves SSH no seu <kbd>computador local</kbd>.

Para fazer isso, podemos usar um utilitário especial chamado <kbd>ssh-keygen</kbd>, que vem incluso com o conjunto padrão de ferramentas do <kbd>OpenSSH</kbd>.

```bash
$ ssh-keygen -t rsa -b 4096
```
{: .nolineno }

Para adicionar sua chave SSH às chaves autorizadas, crie-a <kbd>.ssh</kbd> se ela não estiver criada em seu <kbd>servidor</kbd>.

```bash
 $ mkdir -p ~/.ssh
 ```
{: .nolineno }

Dentro da pasta <kbd>.ssh</kbd> crie o arquivo <kbd>authorized_keys</kbd>. Esse arquivo mantem as chaves publicas autorizadas a fazerem o login.

```bash
$ touch ~/.ssh/authorized_keys && chmod 600 ~/.ssh/authorized_keys
```
{: .nolineno }

Você pode usar o comando ssh-copy-id **(no Linux/macOS)** para transferir facilmente sua chave pública para o <kbd>servidor</kbd>.
Caso voce tenha mudado a porta padrão do ssh, use o parâmetro -p seguido da porta 5050.

```bash
$ ssh-copy-id -p 5050 srv@127.0.0.1
```
{: .nolineno }

Se <kbd>ssh-copy-id</kbd> não estiver disponível, use o comando <kbd>scp</kbd> **(Secure Copy)** para transferir o arquivo da chave pública para o servidor e, em seguida, adicionar a chave ao arquivo <kbd>~/.ssh/authorized_keys</kbd> no servidor.

```bash
$ scp -v -P 5050 ~/.ssh/id_rsa.pub srv@127.0.0.1:/home/srv/.ssh/id_rsa.pub
```
{: .nolineno }

Você pode adicionar o conteúdo do seu arquivo <kbd>id_rsa.pub</kbd> ao final do arquivo <kbd>authorized_keys</kbd>, usando este comando.

```bash
$ cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
```
{: .nolineno }

Para realmente implementar as alterações que acabamos de fazer, reinicie o serviço.

```bash
$ sudo systemctl restart ssh
```
{: .nolineno }

Se tiver completado todos os procedimentos acima, você deve conseguir fazer login no `host remoto` sem a senha da conta.

```bash
$ ssh srv@127.0.0.1
```
{: .nolineno }

### Criando um alias para conectar via SSH

Um alias Bash é um atalho para substituir comandos longos por comandos mais curtos, economizando tempo e evitando digitação repetitiva. Em um ambiente shell, você deve armazenar os <kbd>aliases Bash</kbd> no <kbd>.bashrc</kbd> arquivo sob o diretório inicial do usuário.

```bash
$ echo 'alias sh="ssh -p 5050 srv@127.0.0.1"' >> ~/.bashrc
```
{: .nolineno }

Recarregar as configurações do <kbd>.bashrc</kbd> sem sair do sistema.

```bash
$ source ~/.bashrc
```
{: .nolineno }

Conecte-se ao SSH com o aliases criado

```bash
$ sh
```
{: .nolineno }

# Gerando chaves SSH no Windows - Putty

No Windows você vai precisar do [**Putty**](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html){:target="_blank"} para gerar seu par de chaves pública e privada.

Baixe o instalador com extensão <kbd>.msi</kbd> e instale no seu computador.

Depois de instalado, pressione simultaneamente as teclas <kbd>Windows + R</kbd> e digite <kbd>%programfiles%\Putty</kbd>

> Por padrão a chave privada é gerada com a versão de arquivo `PPK` definida para 3, se for necessário, altere para a versão 2.
{: .prompt-info }

Dentro da pasta do `PuTTY`, abra o arquivo <kbd>puttygen.exe</kbd>. Selecione `RSA` como o tipo de chave para gerar, e digite `4096` clique em <kbd>Generate</kbd> e movimente o mouse dentro do programa próximo a barrinha de carregamento até terminar.

Na imagem abaixo detalho o que você precisa fazer dentro do `PuTTYgen`

![Desktop View](/assets/img/img-blog/ssh-key/putty.png){: .align-center style="width: 650px;" }
_Gerarando chaves com o PuttyGen_

Depois que o <kbd>PuTTYgen</kbd> gerar a chave, selecione toda a área criptografada e dê um <kbd>CTRL + C</kbd> para copiar a chave pública. Anote-a em um bloco de notas.

Além disso, clique no botão <kbd>Save private key</kbd> para salvar a chave gerada. **(Clique em Yes no pop-up que surgir)**.
Salve a chave em um local seguro, ela será importante posteriormente.

Se <kbd>ssh-copy-id</kbd> não estiver disponível, use o comando <kbd>scp</kbd> **(Secure Copy)** para transferir o arquivo da chave pública para o servidor e, em seguida, adicionar a chave ao arquivo <kbd>~/.ssh/authorized_keys</kbd> no **servidor**.

```bash
$ scp -v -P 5050 "C:\Users\User\Desktop\rsa-key.txt" srv@127.0.0.1:/home/srv/.ssh/rsa-key.txt
```
{: .nolineno }

Você pode adicionar o conteúdo do seu arquivo <kbd>rsa-key.txt</kbd> ao final do arquivo <kbd>authorized_keys</kbd>, usando este comando.

```bash
$ cat ~/.ssh/rsa-key.txt >> ~/.ssh/authorized_keys
```
{: .nolineno }

### Configurando o Putty com chave de criptografia

Em <kbd>SSH -> Auth -> Credentials</kbd>, na opção `Private key file for authentication` insira a chave salva na configuração do `puTTYgen`.

![Desktop View](/assets/img/img-blog/ssh-key/cred.png){: width="972" height="589" .w-100 .normal}

Defina os dados de acesso de acordo com suas configurações do `Servidor`

![Desktop View](/assets/img/img-blog/ssh-key/ssh.png){: width="972" height="589" .w-100 .normal}

No menu <kbd>Connection -> Data</kbd>, defina o nome de usuário do `Servidor` que está sendo configurado.

![Desktop View](/assets/img/img-blog/ssh-key/user.png){: width="972" height="589" .w-100 .normal}

Parabéns! Você acabou de criar sua chave e realizar o acesso `SSH` via `Putty`.
