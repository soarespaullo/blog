---
title: Aumentando a Segurança do Servidor SSH
description: Aumentando a Segurança do Servidor SSH
author: soarespaullo
date: 2025-04-18 14:50:00
categories: [Linux, SysAdmin]
tags: [Tutoriais, Servidor, SysAdmin, SSH]
math: true
mermaid: true
image:
  path: /assets/img/img-blog/ssh-key/ssh-key.png
  alt: Aumentando a Segurança do Servidor SSH

---

O SSH <kbd>(Secure Socket Shell)</kbd> é normalmente o principal meio para acesso aos servidores, especialmente aqueles ambientes que rodam sistemas Unix-like. Apesar de ter um nível de segurança agradável, é necessário o ajuste de alguns parâmetros.

Para alterar e fortalecer as configurações do servidor SSH, você precisa editar o arquivo de configuração principal do OpenSSH <kbd>/etc/ssh/sshd_config.</kbd> Cada linha não comentada no arquivo representa uma configuração ativa que você pode definir para corresponder às suas preferências.

> Antes de fazer alterações na configuração do servidor SSH, faça backup do arquivo
{: .prompt-info }

```bash
$ sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bkp
```
{: .nolineno }

O primeiro passo para configurar a autenticação de chaves SSH para seu servidor é gerar um par de chaves SSH no seu **computador local**.

Para fazer isso, podemos usar um utilitário especial chamado <kbd>ssh-keygen</kbd>, que vem incluso com o conjunto padrão de ferramentas do <kbd>OpenSSH.</kbd>

```bash
$ ssh-keygen -t rsa -b 4096
```
{: .nolineno }

Para adicionar sua chave SSH às chaves autorizadas, crie-a <kbd>.ssh</kbd> se ela não estiver criada em seu **servidor**

```bash
 $ mkdir -p ~/.ssh
 ```
{: .nolineno }

Dentro da pasta <kbd>.ssh</kbd> crie o arquivo <kbd>authorized_keys</kbd>. Esse arquivo mantem as chaves publicas autorizadas a fazerem o login.

```bash
$ touch ~/.ssh/authorized_keys && chmod 600 ~/.ssh/authorized_keys
```
{: .nolineno }

Você pode usar o comando <kbd>ssh-copy-id</kbd> **(no Linux/macOS)** para transferir facilmente sua chave pública para o servidor.

```bash
$ ssh-copy-id -p 5050 admin@127.0.0.1
```
{: .nolineno }

Se <kbd>ssh-copy-id</kbd> não estiver disponível, pode usar o comando <kbd>scp</kbd> **(Secure Copy)** para transferir o arquivo da chave pública para o servidor e, em seguida, adicionar a chave ao arquivo <kbd>~/.ssh/authorized_keys</kbd> no servidor.

```bash
$ scp -v -P 5050 ~/.ssh/id_rsa.pub root@127.0.0.1:/home/cloud/id_rsa.pub
```
{: .nolineno }

Você pode adicionar o conteúdo do seu arquivo <kbd>id_rsa.pub</kbd> ao final do arquivo <kbd>authorized_keys</kbd>, usando este comando

```bash
$ cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
```
{: .nolineno }

Para realmente implementar as alterações que acabamos de fazer, reinicie o serviço.

```bash
$ sudo systemctl restart ssh
```
{: .nolineno }

# Alterando a Porta padrão do SSH

A porta padrão do SSH é a 22 e por conta disso muitos ataques são direcionado a ela, para mitigar esse problema, iremos mudar para a porta 5050.

> Essa medida não fará com que a porta nunca seja descoberta, e isso é facilmente demonstrado com um Port Scanner.
{: .prompt-warning }

```bash
$ sudo sed -i 's/#Port 22/Port 5050/' /etc/ssh/sshd_config
```
{: .nolineno }

# Desative o login como ROOT

Impeça que o usuário root consiga fazer o login via SSH, tome como boa prática a utilização de um usuário sem privilégio administrativo para acessar o servidor.

Procure no arquivo de configuração o parâmetro <kbd>PermitRootLogin</kbd> e defina como <kbd>“no“.</kbd>

```bash
$ sudo sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin no/' /etc/ssh/sshd_config
```
{: .nolineno }

# Definindo o número máximo das tentativas de acesso

Durante um ataque de força bruta é normal que várias tentativas de acesso sejam feitas, para mitigar esse problema vamos definir como 5. Fique à vontade para definir o número de tentativas que quiser.

Procure no arquivo de configuração o parâmetro <kbd>MaxAuthTries</kbd> e defina como <kbd>“5“.</kbd>

```bash
$ sudo sed -i 's/#MaxAuthTries 6/MaxAuthTries 5/' /etc/ssh/sshd_config
```
{: .nolineno }

# Bloqueando o login de um usuário sem senha

O parâmetro <kbd>PermitEmptyPasswords</kbd> especifica se o servidor SSH vai permitir o login de contas com sequências de senha vazias.

Para evitar o acesso remoto do shell por contas que possuem uma senha vazia, é preciso modificar o parâmetro da configuração, reduzindo as chances de acesso não autorizado ao sistema.

Procure no arquivo de configuração o parâmetro <kbd>PermitEmptyPasswords</kbd> e defina como <kbd>“no“.</kbd>

```bash
$ sudo sed -i 's/#PermitEmptyPasswords no/PermitEmptyPasswords no/' /etc/ssh/sshd_config
```
{: .nolineno }

# Desabilite o X11Forwarding

O <kbd>X11Forwarding</kbd> permite a execução de programas em modo gráfico via SSH, e dependendo da configuração o acesso com permissão ilimitada pode acontecer, se você não tem necessidade de usar, desabilite.

Procure no arquivo de configuração o parâmetro <kbd>X11Forwarding</kbd> e defina como <kbd>“no“.</kbd>

```bash
$ sudo sed -i 's/#X11Forwarding no/X11Forwarding no/' /etc/ssh/sshd_config
```
{: .nolineno }

# Conceder Acesso SSH a Usuários Autorizados

Para permitir apenas os usuários admin e guest efetuem login via SSH, adicione <kbd>AllowUsers</kbd> no final do arquivo

```bash
AllowUsers admin guest
```
{: .nolineno }
 
A regra acima permite admin e guest a fazer login a partir de qualquer endereço IP

Para permitir que usuários específicos efetuem login apenas a partir de endereços IP específicos, adicione a seguinte diretiva

```bash
AllowUsers admin@127.0.0.1 guest@127.0.0.2 
```
{: .nolineno }

# Limitar o Número de Sessões Ativas por Usuário

Às vezes, usuários SSH podem abrir no máximo 5 conexões SSH, dependendo das operações em andamento. No entanto, às vezes, invasores podem iniciar sessões extras durante ataques <kbd>MITM (Man in the Middle).</kbd> Para aumentar a segurança do seu servidor, limite o número de sessões ativas por usuários.

```bash
$ sudo sed -i 's/#MaxSessions 10/MaxSessions 5/' /etc/ssh/sshd_config
```
{: .nolineno }

# Desativar logins com senha

Em comparação com as chaves SSH, os invasores usam força bruta em servidores SSH usando senhas. Desabilite logins baseados em senha para todos os usuários do sistema.

Para desabilitar logins baseados em senha e aceitar apenas chaves SSH, encontre a seguinte linha <kbd>PasswordAuthentication yes</kbd>. Por padrão, ele é definido como yes, altere para <kbd>"no".</kbd>
 
```bash
$ sudo sed -i 's/PasswordAuthentication yes/PasswordAuthentication no/' /etc/ssh/sshd_config
```
{: .nolineno }

# Autenticar-se em seu servidor usando chaves SSH

Se tiver completado todos os procedimentos acima, você deve conseguir fazer login no host remoto sem a senha da conta.

```bash
$ ssh admin@127.0.0.1
```
{: .nolineno }

# Bash Aliases

Um alias no Bash é uma maneira de executar um comando longo usando um curto.

```bash
$ echo 'alias sh="ssh -p 5050 root@127.0.0.1"' >> ~/.bashrc
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
