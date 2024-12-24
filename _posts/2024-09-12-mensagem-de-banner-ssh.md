---
title: Mensagem de Banner em SSH 
description: Mensagem de banner em servidores Linux através do SSH.
author: soarespaullo
date: 2024-09-12 22:20:00
categories: [Linux]
tags: [Tutoriais, putty, ssh]
pin: true
math: true
mermaid: true
image:
  path: /assets/img/img-blog/ssh/secure-shell.png
  alt: Mensagem de banner em servidores Linux através do SSH.

---

Hoje, irei mostrar uma funcionalidade muito bacana e nativa que existe no arquivo de configuração do protocolo SSH.

Trata-se de um banner simples que é apresentado na tela de login do usuário após o mesmo tentar se conectar com um servidor Linux utilizando o protocolo SSH.

![Desktop View](/assets/img/img-blog/ssh/web.png)

Para que possamos obter o mesmo resultados da imagem basta seguir os passos abaixo:

1° Crie o arquivo <kbd>(ssh-banner.txt)</kbd> dentro do diretório <kbd>/etc/ssh</kbd>

```bash
$ sudo touch /etc/ssh/ssh-banner.txt
```
{: .nolineno }

Após criar o arquivo com touch, basta apenas inserir algum texto dentro do arquivo. Esse texto ira aparecer na tela de login do SSH assim que o usuário tentar o acesso.

Algumas dicas:

Você pode adicionar uma mensagem de alerta para o seu servidor, algo do tipo “Acesso não permitido, apenas pessoas autorizadas”.

Ou você pode apenas personalizar como eu fiz no exemplo do “Servidor Web”, deixarei o link do site que deixou as letras daquele jeito:

#### [**Texto para ASCII**](https://patorjk.com/software/taag/#p=display&f=Graffiti&t=Type%20Something%20){:target="_blank"}.

Nesse site, você consegue transformar qualquer texto em uma arte simples, depois é só copiar e colar dentro do seu arquivo (ssh-banner.txt)

Terminado o 1° passo , agora é onde de fato iremos habilitar essa funcionalidade ao SSH.

2° Acesse o arquivo de configuração do SSH chamado <kbd>“sshd_config”</kbd> que fica em <kbd>/etc/ssh</kbd>

```bash
$ sudo vim /etc/ssh/sshd_config
```
{: .nolineno }

Dentro do arquivo <kbd>“sshd_config”</kbd> adicione a seguinte linha <kbd>“Banner /etc/ssh/ssh-banner.txt”</kbd>

![Desktop View](/assets/img/img-blog/ssh/ssh.png)

3° Após adicionar a linha basta apenas salvar e reiniciar o serviço do SSH

Para reiniciar o serviço do SSH;

```bash
$ sudo systemctl restart ssh
```
{: .nolineno }

Depois é só testar fazendo um acesso remoto via terminal, putty, cmd etc.. 🙂
