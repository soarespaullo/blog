---
title: Exemplo de instalação no Ubuntu 22.04 LTS
description: Iniciando a Instalação/Configuração - Ubuntu 22.04 LTS
author: soarespaullo
date: 2024-09-13 23:20:00
categories: [Hacking, Cloud, Blog]
tags: [Tutoriais, Cloud, Nextcloud]
math: true
mermaid: true
image:
  path: /assets/img/img-blog/nextcloud.png
  alt: Iniciando a Instalação/Configuração - Ubuntu 22.04 LTS

---

Iniciando a Instalação/Configuração - Ubuntu 22.04 LTS

```bash
$ sudo apt update && sudo apt upgrade

$ sudo apt install apache2 mariadb-server libapache2-mod-php php-gd php-mysql \

php-curl php-mbstring php-intl php-gmp php-bcmath php-xml php-imagick imagemagick php-zip
```
{: .nolineno }

> Para iniciar o modo de linha de comando do <kbd>MySQL</kbd>, use o seguinte comando e pressione a tecla Enter quando for solicitada uma senha:

```bash
$ sudo mysql
```
{: .nolineno }

> Em seguida, um prompt <kbd>MariaDB [root]></kbd> aparecerá. Agora insira as seguintes linhas, substituindo o nome de usuário e a senha pelos valores apropriados, e confirme-os com a tecla Enter:

CREATE USER 'nextcloud'@'localhost' IDENTIFIED BY 'P@ssW0rDs2';

CREATE DATABASE IF NOT EXISTS nextcloud CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;

GRANT ALL PRIVILEGES ON nextcloud.* TO 'nextcloud'@'localhost';

FLUSH PRIVILEGES;

Você pode sair do <kbd>prompt</kbd> digitando:

quit;

**AVISO:** Os campos que estiverem o nome <kbd>nextcloud e P@ssW0rDs2</kbd>, mudar de acordo com sua preferência.

#### Agora baixe o arquivo da última versão do Nextcloud

Vá para a [página de download do Nextcloud](https://nextcloud.com/install){:target="_blank"}.

Vá para : Get Nextcloud > Nextcloud Server > Community Projects e baixe o arquivo tar.bz2 ou .zip.
Isso baixa um arquivo chamado nextcloud-xyztar.bz2 ou nextcloud-xyz.zip (onde xyz é o número da versão).
Baixe seu arquivo de soma de verificação correspondente, por exemplo, nextcloud-xyztar.bz2.md5 ou nextcloud-xyztar.bz2.sha256.
Verifique a soma MD5 ou SHA256:
Se preferir, pode baixar usando o wget;
wget https://download.nextcloud.com/server/releases/nextcloud-27.1.3.zip
https://download.nextcloud.com/server/releases/nextcloud-27.1.3.zip.md5







