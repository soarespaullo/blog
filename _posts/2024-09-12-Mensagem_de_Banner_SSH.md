---
title: Mensagem de Banner em SSH 
description: Mensagem de banner em servidores Linux através do SSH.
author: soarespaullo
date: 2020-09-16 20:55:00 -0300
categories: [Hacking]
tags: [Tutoriais]
pin: true
math: true
mermaid: true
image:
  path: /assets/img/img-blog/banner.png
  alt: Mensagem de banner em servidores Linux através do SSH.

---

Hoje, irei mostrar uma funcionalidade muito bacana e nativa que existe no arquivo de configuração do protocolo SSH.

Trata-se de um banner simples que é apresentado na tela de login do usuário após o mesmo tentar se conectar com um servidor Linux utilizando o protocolo SSH.

![Desktop View](/assets/img/img-blog/web.png)

Para que possamos obter o mesmo resultados da imagem basta seguir os passos abaixo:

1° Crie o arquivo (ssh-banner.txt) dentro do diretório /etc/ssh

```bash
$ sudo touch /etc/ssh/ssh-banner.txt
```

Após criar o arquivo com touch, basta apenas inserir algum texto dentro do arquivo. Esse texto ira aparecer na tela de login do SSH assim que o usuário tentar o acesso.

Algumas dicas:

Você pode adicionar uma mensagem de alerta para o seu servidor, algo do tipo “Acesso não permitido, apenas pessoas autorizadas”.

Ou você pode apenas personalizar como eu fiz no exemplo do “Servidor Web”, deixarei o link do site que deixou as letras daquele jeito:

#### [**Text to ASCII**](https://patorjk.com/software/taag/#p=display&f=Graffiti&t=Type%20Something%20)

Nesse site, você consegue transformar qualquer texto em uma arte simples, depois é só copiar e colar dentro do seu arquivo (ssh-banner.txt)

Terminado o 1° passo , agora é onde de fato iremos habilitar essa funcionalidade ao SSH.

2° Acesse o arquivo de configuração do SSH chamado “sshd_config” que fica em /etc/ssh

```bash
$ sudo vim /etc/ssh/sshd_config
```

Dentro do arquivo “sshd_config” adicione a seguinte linha “Banner /etc/ssh/ssh-banner.txt”

![Desktop View](/assets/img/img-blog/ssh.png)

3° Após adicionar a linha basta apenas salvar e reiniciar o serviço do SSH

Para reiniciar o serviço do SSH;

```bash
$ sudo systemctl restart ssh
```

Depois é só testar fazendo um acesso remoto via terminal, putty, cmd etc.. 🙂
