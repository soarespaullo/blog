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

## Iniciando a Instalação/Configuração - Ubuntu 22.04 LTS

```bash
$ sudo apt update && sudo apt upgrade

$ sudo apt install apache2 mariadb-server libapache2-mod-php php-gd php-mysql \

php-curl php-mbstring php-intl php-gmp php-bcmath php-xml php-imagick imagemagick php-zip
```
{: .nolineno }

Para iniciar o modo de linha de comando do <kbd>MySQL</kbd>, use o seguinte comando e pressione a tecla enter quando for solicitada uma senha:

```bash
$ sudo mysql
```
{: .nolineno }

Em seguida, um prompt <kbd>MariaDB [root]></kbd> aparecerá. Agora insira as seguintes linhas, substituindo o nome de usuário e a senha pelos valores apropriados, e confirme-os com a tecla Enter:

```bash
CREATE USER 'nextcloud'@'localhost' IDENTIFIED BY 'P@ssW0rDs2';

CREATE DATABASE IF NOT EXISTS nextcloud CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;

GRANT ALL PRIVILEGES ON nextcloud.* TO 'nextcloud'@'localhost';

FLUSH PRIVILEGES;
```
{: .nolineno}

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

Agora você pode extrair e copiar o conteúdo para o diretório raiz do Apache.

```bash
$ sudo rm -rf /var/www/html/
$ sudo unzip nextcloud-x.y.z.zip -d /var/www
```

Configurar o Apache requer a criação de um único arquivo de configuração. No Debian, Ubuntu e seus derivados, este arquivo será;

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

Editando <kbd>config</kbd> e adicionando parâmetros para forçar o <kbd>SSL</kbd>.

```bash
$ sudo vim /var/www/nextcloud/config/config.php

'forcessl' => true,

'forceSSLforSubdomains' => true,
```
{: .nolineno}


#### Redirecionar HTTP para HTTPS

Edite o <kbd>nexcloud.conf</kbd> e adicione o <kbd>redirect</kbd>.

```bash
$ sudo vim /etc/apache2/sites-available/nextcloud.conf
```
{: .nolineno}

[**Redirect permanent** / https://cloud.nextcloud.com/](https://cloud.nextcloud.com/){:target="_blank"}

#### Certificado Let's Encrypt

Para obter um certificado e fazer com que o Certbot edite sua configuração do apache automaticamente.

```bash
$ sudo apt install certbot certbot-apache2
$ sudo certbot --apache
```

#### Renovar Individual

```bash
$ sudo certbot certonly --manual -d seu.cloud.com
```
{: .nolineno}

#### Arquivo de Configuração de Renovação Automática

**AVISO:** Não precisa configurar, porque já no ato da instalação ele já cria um arquivo no <kbd>cron</kbd>

```bash
$ sudo vim /etc/cron.d/certbot 
```
{: .nolineno}

#### Aplicando as configurações

```bash
$ sudo a2ensite /etc/apache2/sites-available/nextcloud-le-ssl.conf
```
{: .nolineno}

#### Ativar Segurança de Transporte Estrito HTTP

Embora redirecionar todo o tráfego para <kbd>HTTPS</kbd> seja bom, pode não impedir completamente os ataques <kbd>Man-In-The-Middle.</kbd>

Isso pode ser obtido definindo as seguintes configurações no arquivo Apache <kbd>VirtualHost:</kbd>

```bash
$ sudo vim /etc/apache2/sites-available/nextcloud-le-ssl.conf
```
{: .nolineno}

```sass
<VirtualHost *:443>
  ServerName seu.nextcloud.com
    <IfModule mod_headers.c>
      Header always set Strict-Transport-Security "max-age=15552000; includeSubDomains"
    </IfModule>
 </VirtualHost>
```
{: file='/etc/apache2/sites-available/nextcloud-le-ssl.conf'}
{: .nolineno}

#### Assistente de instalação - Pelo Browser

Após reiniciar o Apache, você deve concluir a instalação executando o Assistente de Instalação gráfico ou na linha de comando com o <kbd>occ</kbd> comando. Para habilitar isso, mude a propriedade em seus diretórios Nextcloud para seu usuário HTTP:

```bash
$ sudo chown -R www-data:www-data /var/www/nextcloud/
```
{: .nolineno}

Agora você deve conseguir acessar sua instância **Nextcloud** navegando até <kbd>http://YOUR-SERVER-LOCAL-IP</kbd> ou [https://cloud.nextcloud.com](https://cloud.nextcloud.com)

#### URLs Bonitos

Os **URLs bonitos** removem a <kbd>(index.php)</kbd> parte-em todos os URLs do Nextcloud.
<kbd>(mod_env)</kbd> e <kbd>(mod_rewrite)</kbd> deve ser instalado/ativados em seu servidor web e <kbd>.htaccess</kbd> deve ser gravável pelo usuário <kbd>HTTP</kbd>. Então você pode definir as <kbd>(config.php)</kbd> duas variáveis:

```bash
$ sudo vim /var/www/nextcloud/config/config.php
```
{: .nolineno}

> 'overwrite.cli.url' => 'https://cloud.nextcloud.com/',
{: .prompt-info }

> 'htaccess.RewriteBase' => '/',
{: .prompt-info }

Se não estiver instalado em uma subpasta. Por fim, execute este comando <kbd>occ</kbd> para atualizar seu arquivo <kbd>.htaccess:</kbd>

```bash
$ sudo -u www-data php /var/www/nextcloud/occ maintenance:update:htaccess
```
{: .nolineno}

#### Crontab

Isso executará o cronjob do Nextcloud a cada 5 minutos - 
[**https://crontab.guru/every-5-minutes**](https://crontab.guru/every-5-minutes)

Use o serviço cron do sistema para chamar o arquivo <kbd>cron.php</kbd> a cada 5 minutos. O <kbd>cron.php</kbd> precisa ser executado pelo usuário de sistema "www-data"

```bash
$ sudo crontab -u www-data -e
```
{: .nolineno}

Se solicitado, pressione “1” para usar o editor nano (que é muito fácil de usar) e adicione a seguinte linha ao seu arquivo crontab:

```bash
*/5 * * * * php -f /var/www/nextcloud/cron.php
```
{: .nolineno}

Depois ir em: <kbd>configurações</kbd> , configurações básicas e <kbd>cron</kbd>.

#### Habilitar Cache - (Redis)

No Debian / Ubuntu / Mint, instale <kbd>redis-server</kbd> e <kbd>php-redis</kbd>. O instalador irá iniciar automaticamente <kbd>redis-servere</kbd> configurá-lo para iniciar na inicialização.

```bash
$ sudo apt install php-redis redis-server
```
{: .nolineno}

Você pode verificar se o daemon Redis está sendo executado com: ps ax

```bash
$ ps ax | grep redis
```
{: .nolineno}

Depois, abra o arquivo de configuração do Redis em /etc/redis/redis.conf

```bash
$ sudo vim /etc/redis/redis.conf
```
{: .nolineno}

Agora, encontre e altere:

```bash
port 6379 para port 0
```
{: .nolineno}

Em seguida, descomente:

```bash
unixsocket /var/run/redis/redis.sock
unixsocketperm 700 alterando as permissões para 770 ao mesmo tempo:
unixsocketperm 770
```
{: .nolineno}

Salve e saia, em seguida, adicione o usuário <kbd>Apache www-data ao redis grupo</kbd>:

```bash
sudo usermod -a -G redis www-data
```
{: .nolineno}

Finalmente, reinicie o Apache com:

```bash
$ sudo systemctl restart apache2.service
```
{: .nolineno}

Com Redis configurado, adicione;

```bash
$ sudo vim /var/www/nextcloud/config/config.php

'memcache.local' => '\OC\Memcache\Redis',

'memcache.distributed' => '\OC\Memcache\Redis',

'redis' => [

  'host'   => '/var/run/redis/redis-server.sock',

  'port'   => 0,

], 
```
{: .nolineno}

**AVISO:** Se der algum erro ou não carregar a página, reinicie o  seu servidor...


#### Fornecimento de arquivos padrão

Você pode distribuir um conjunto de arquivos e pastas padrão para todos os usuários.

```bash
$ sudo mkdir -p /media/cloud/core/skeleton

$ sudo mkdir -p /media/cloud/core/templates

$ sudo vim /var/www/nextcloud/config/config.php

'skeletondirectory' => '/media/cloud/core/skeleton',
```
{: .nolineno}


#### Para arquivos de Modelos.

```bash
'templatedirectory' => '/media/cloud/core/templates',

$ sudo chown -R www-data:www-data /media/cloud/core/skeleton/

$ sudo systemctl restart apache2.service 
```
{: .nolineno}

Deixe em branco para não copiar nenhum arquivo de modelo.

**AVISO:** Substituir os arquivos <kbd>(core/skeleton)</kbd> não é recomendado, porque essas alterações serão substituídas na próxima atualização do servidor Nextcloud.

#### Mover diretório de dados Nextcloud

**AVISO:** Certifique-se de que a pasta “data” no diretório de destino ainda não exista!

```bash
$ sudo mkdir -p /media/cloud/data

$ sudo cp -a /var/www/nextcloud/data/. /media/cloud/data/
```
{: .nolineno}

#### Em seguida, atualize a configuração do Nextcloud para refletir o novo diretório de dados:

```bash
$ sudo vim /var/www/nextcloud/config/config.php

'datadirectory' => '/media/cloud/data',

$ sudo chown -R www-data:www-data /media/cloud/data

$ sudo rm -rf /var/www/nextcloud/data
```
{: .nolineno}

#### Finalizar Sessão

```bash
$ sudo vim /var/www/nextcloud/config/config.php

'session_lifetime' => 3600,

'session_keepalive' => false,

'remember_login_cookie_lifetime' => 0,
```
{: .nolineno}


#### Isso define o idioma padrão em seu servidor Nextcloud

```bash
$ sudo vim /var/www/nextcloud/config/config.php 

'default_language' => 'pt_BR',

'force_language' => 'pt_BR',

'default_locale' => 'pt_BR',

'force_locale' => 'pt_BR',
```
{: .nolineno}

**AVISO:** Se a configuração: <kbd>'force_language'</kbd> for forçado, os usuários também não poderão alterar seu idioma nas configurações pessoais. Se os usuários não conseguirem alterar seu idioma, mas os usuários tiverem idiomas diferentes, esse valor pode ser definido como em true vez de um código de idioma.


#### Habilitar .htaccess

O <kbd>.htaccess</kbd> arquivo não funciona porque colocamos Nextcloud na <kbd>/var/www/</kbd> principal controlada pelo <kbd>apache2.conf</kbd> arquivo. Por padrão, ele é definido para não permitir <kbd>.htaccess</kbd> substituições e vamos precisar mudar isso:

```bash
$ sudo vim /etc/apache2/apache2.conf
```
{: .nolineno}

De;

```bash
<Directory /var/www/>     

    Options Indexes FollowSymLinks

    AllowOverride None  

    Require all granted

</Directory>
```
{: .nolineno}

Para;

```bash
<Directory /var/www/>

    Options Indexes FollowSymLinks

    AllowOverride All

    Require all granted

</Directory>
```
{: .nolineno}
