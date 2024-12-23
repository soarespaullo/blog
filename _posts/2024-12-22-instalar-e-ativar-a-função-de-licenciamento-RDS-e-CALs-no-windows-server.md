---
title: Como instalar e ativar a função de licenciamento RDS e CALs no Windows Server
description: Instalar a Função de Licenciamento de Área de Trabalho Remota no Windows Server
author: soarespaullo
date: 2024-12-22 17:50:00
categories: [Windows, SysAdmin]
tags: [Tutoriais, Windows,Servidor, SysAdmin, RDS, CALs]
math: true
mermaid: true
image:
  path: /assets/img/img-blog/windows-server.png
  alt: Instalar a Função de Licenciamento de Área de Trabalho Remota no Windows Server

---

O **servidor de Licenciamento de Área de Trabalho Remota** é usado para emitir e rastrear licenças de acesso de terminal de cliente RDS ( **CALs** ). A política de licenciamento da Microsoft exige que todos os usuários ou dispositivos que se conectam a sessões de área de trabalho no RDS sejam licenciados. Neste artigo, veremos como instalar e ativar a função de servidor de Licenciamento de Área de Trabalho Remota em um host executando o Windows Server 2022, 2019 ou 2016 e como instalar CALs RDS.

Instale o serviço de Licenciamento de Área de Trabalho Remota no console do Gerenciador do Servidor <kbd>(Adicionar Funções e Recursos -> Serviços de Área de Trabalho Remota -> Licenciamento de Área de Trabalho Remota).</kbd>

![Desktop View](/assets/img/img-blog/licenciamentord.png)

Aguarde a instalação da função.

![Desktop View](/assets/img/img-blog/finalizado.png)

## Ative o Servidor de Licenças dos Serviços de Área de Trabalho Remota

Para emitir licenças para clientes RDP, seu Servidor de Licença RDS deve ser ativado. Abra o **Gerenciador de Licenciamento de Área de Trabalho Remota** <kbd>licmgr.exe</kbd>, clique com o botão direito do mouse no nome do seu servidor e selecione Ativar Servidor .

![Desktop View](/assets/img/img-blog/ativar.png)

No assistente de ativação do servidor de licenciamento RDS, escolha se deseja ativar o servidor pela Internet, usando um navegador ou por telefone.

![Desktop View](/assets/img/img-blog/ativacao.png)

Em seguida, preencha algumas informações sobre sua empresa **(alguns campos são obrigatórios)**.

![Desktop View](/assets/img/img-blog/informacao.png)

Clique no botão Finalizar . A seguinte mensagem deve aparecer:

> O servidor de licença foi ativado com sucesso!
{: .prompt-info }

![Desktop View](/assets/img/img-blog/concluido.png)

Clique no nome do servidor no console e selecione **Examinar Configurações** . Neste exemplo, o servidor de licença RD está habilitado e pode emitir licenças para clientes no domínio do AD.

> OEste servidor de licença é um membro do grupo Terminal Server License Servers no Active Directory. Este servidor de licença poderá emitir RDS Per User CALs para usuários no domínio, e você poderá rastrear o uso de RDS Per User CALs.
{: .prompt-info }

> Este servidor de licença é registrado como um ponto de conexão de serviço (SCP) nos Serviços de Domínio do Active Directory.
{: .prompt-info }
