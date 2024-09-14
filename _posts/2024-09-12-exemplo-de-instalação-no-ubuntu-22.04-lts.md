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

- Vá para a [página de download do Nextcloud](https://nextcloud.com/install){:target="_blank"}.

- Vá para : Get Nextcloud > Nextcloud Server > Community Projects e baixe o arquivo tar.bz2 ou .zip.
- Isso baixa um arquivo chamado nextcloud-xyztar.bz2 ou nextcloud-xyz.zip (onde xyz é o número da versão).
- Baixe seu arquivo de soma de verificação correspondente, por exemplo, nextcloud-xyztar.bz2.md5 ou nextcloud-xyztar.bz2.sha256.
- Verifique a soma MD5 ou SHA256:
- Se preferir, pode baixar usando o wget;
- wget https://download.nextcloud.com/server/releases/nextcloud-27.1.3.zip
- https://download.nextcloud.com/server/releases/nextcloud-27.1.3.zip.md5

```bash
md5sum -c nextcloud-x.y.z.zip.md5 < nextcloud-x.y.z.zip
```
{: .nolineno}

> Agora você pode extrair e copiar o conteúdo para o diretório raiz do Apache.

```bash
$ sudo rm -rf /var/www/html/
$ sudo unzip nextcloud-x.y.z.zip -d /var/www
```

> Configurar o Apache requer a criação de um único arquivo de configuração. No Debian, Ubuntu e seus derivados, este arquivo será;

```bash
$ sudo touch /etc/apache2/sites-available/nextcloud.conf
$ sudo vim /etc/apache2/sites-available/nextcloud.conf
```

Para utilizar a instalação <kbd>host virtual</kbd>, coloque o seguinte em seu <kbd>nextcloud.conf</kbd> substituindo <kbd>ServerName</kbd>, bem como os <kbd>DocumentRoot</kbd> e Diretório com valores apropriados para o seu sistema:

```sass
<VirtualHost *:80>
  DocumentRoot /var/www/nextcloud/
  ServerName  seu.server.com

  <Directory /var/www/nextcloud/>
    Require all granted
    AllowOverride All
    Options FollowSymLinks MultiViews

    <IfModule mod_dav.c>
      Dav off
    </IfModule>

  </Directory>
</VirtualHost>
```
{: file='/etc/apache2/sites-available'}
{: .nolineno}

#### Ajustando o Apache

```bash
$ sudo a2dissite 000-default.conf
$ sudo a2ensite nextcloud.conf
$ sudo systemctl restart apache2
```

#### Configurações adicionais do Apache

```bash
$ sudo a2enmod dir env headers mime rewrite ssl
$ sudo systemctl restart apache2
```

#### Configurar NextCloud para SSL

> Editando **config** e adicionando parâmetros para forçar o **SSL**.

```bash
$ sudo vim /var/www/nextcloud/config/config.php

'forcessl' => true,

'forceSSLforSubdomains' => true,
```
{: .nolineno}


#### Redirecionar HTTP para HTTPS

> Edite o nexcloud.conf e adicione o redirect.

```bash
$ sudo vim /etc/apache2/sites-available/nextcloud.conf

 [**Redirect permanent** /](https://cloud.nextcloud.com/){:target="_blank"}
```
{: .nolineno}

#### Certificado Let's Encrypt

> Para obter um certificado e fazer com que o Certbot edite sua configuração do apache automaticamente.

```bash
$ sudo apt install certbot certbot-apache2
$ sudo certbot --apache
```

#### Renovar Individual

```bash
$ sudo certbot certonly --manual -d seu.cloud.com
```
{: .nolineno}
