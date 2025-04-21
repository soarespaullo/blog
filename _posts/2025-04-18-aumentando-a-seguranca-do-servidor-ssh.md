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

Para alterar e fortalecer as configurações do servidor SSH, você precisa editar o arquivo de configuração principal do OpenSSH <kbd>/etc/ssh/sshd_config</kbd>. Cada linha não comentada no arquivo representa uma configuração ativa que você pode definir para corresponder às suas preferências.

> Antes de fazer alterações na configuração do servidor SSH, faça `backup` do arquivo: `/etc/ssh/sshd_config`.
{: .prompt-info }

### Alterando a porta padrão do SSH

A porta padrão do SSH é a 22 e por conta disso muitos ataques são direcionado a ela, para mitigar esse problema, iremos mudar para a porta 5050.

> A mudança da porta padrão, como a porta `22` do `SSH`, para outra não garante que a porta seja permanentemente escondida, pois o port scanning (ou varredura de portas) é uma ferramenta comum usada para identificar portas abertas em um sistema.
{: .prompt-warning }

```bash
$ sudo sed -i 's/#Port 22/Port 5050/' /etc/ssh/sshd_config
```
{: .nolineno }

### Desativar logins com senha

Em comparação com as chaves SSH, os invasores usam força bruta em servidores SSH usando senhas. Desabilite logins baseados em senha para todos os usuários do sistema.

Para desabilitar logins baseados em senha e aceitar apenas chaves SSH, encontre a seguinte linha <kbd>PasswordAuthentication yes</kbd>. Por padrão, ele é definido como <kbd>yes</kbd>, altere para <kbd>"no".</kbd>
 
```bash
$ sudo sed -i 's/PasswordAuthentication yes/PasswordAuthentication no/' /etc/ssh/sshd_config
```
{: .nolineno }

### Habilitando autenticação de chave pública SSH

No servidor, verifique se a configuração do SSH permite autenticação por chave pública. Edite o arquivo de configuração do SSH <kbd>/etc/ssh/sshd_config</kbd>

```bash
$ sudo sed -i 's/#PubkeyAuthentication yes/PubkeyAuthentication yes/' /etc/ssh/sshd_config
```
{: .nolineno }

### Desative o login como root

Impeça que o usuário root consiga fazer o login via SSH, tome como boa prática a utilização de um usuário sem privilégio administrativo para acessar o servidor.

Procure no arquivo de configuração o parâmetro <kbd>PermitRootLogin</kbd> e defina como <kbd>“no“.</kbd>

```bash
$ sudo sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin no/' /etc/ssh/sshd_config
```
{: .nolineno }

### Definindo o número máximo das tentativas de acesso

Durante um ataque de força bruta é normal que várias tentativas de acesso sejam feitas, para mitigar esse problema vamos definir como 5. Fique à vontade para definir o número de tentativas que quiser.

Procure no arquivo de configuração o parâmetro <kbd>MaxAuthTries</kbd> e defina como <kbd>“5“.</kbd>

```bash
$ sudo sed -i 's/#MaxAuthTries 6/MaxAuthTries 5/' /etc/ssh/sshd_config
```
{: .nolineno }

### Bloqueando o login de um usuário sem senha

O parâmetro <kbd>PermitEmptyPasswords</kbd> especifica se o servidor SSH vai permitir o login de contas com sequências de senha vazias.

Procure no arquivo de configuração o parâmetro <kbd>PermitEmptyPasswords</kbd> e defina como <kbd>“no“.</kbd>

```bash
$ sudo sed -i 's/#PermitEmptyPasswords no/PermitEmptyPasswords no/' /etc/ssh/sshd_config
```
{: .nolineno }

### Desabilite o X11Forwarding

O <kbd>X11Forwarding</kbd> permite a execução de programas em modo gráfico via SSH, e dependendo da configuração o acesso com permissão ilimitada pode acontecer, se você não tem necessidade de usar, desabilite.

Procure no arquivo de configuração o parâmetro <kbd>X11Forwarding</kbd> e defina como <kbd>“no“.</kbd>

```bash
$ sudo sed -i 's/X11Forwarding yes/X11Forwarding no/' /etc/ssh/sshd_config
```
{: .nolineno }

### Conceder acesso SSH a usuários autorizados

Para permitir apenas os usuários admin e guest efetuem login via SSH, adicione <kbd>AllowUsers</kbd> no final do arquivo

```bash
AllowUsers admin guest
```
{: .nolineno }
 
> A regra acima permite `ADMIN` e `GUEST` a fazer login a partir de qualquer endereço `IP`
{: .prompt-warning }

Para permitir que usuários específicos efetuem login apenas a partir de endereços IP específicos, adicione o seguinte.

```bash
AllowUsers admin@127.0.0.1 guest@127.0.0.2 
```
{: .nolineno }

### Limitar o número de sessões ativas por usuário

Às vezes, usuários SSH podem abrir no máximo 5 conexões SSH, dependendo das operações em andamento. No entanto, às vezes, invasores podem iniciar sessões extras durante ataques <kbd>MITM (Man in the Middle)</kbd>. Para aumentar a segurança do seu servidor, limite o número de sessões ativas por usuários.

```bash
$ sudo sed -i 's/#MaxSessions 10/MaxSessions 5/' /etc/ssh/sshd_config
```
{: .nolineno }

Neste artigo, abordamos as etapas importantes necessárias para aumentar a segurança do sistema e configurar a autenticação de chave pública entre um computador local e um servidor remoto. Também vimos como podemos desabilitar a autenticação tradicional baseada em senha para proteger nosso servidor remoto de ataques de força bruta.
