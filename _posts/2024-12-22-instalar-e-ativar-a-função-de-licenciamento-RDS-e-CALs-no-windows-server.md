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

Instale o serviço de Licenciamento de Área de Trabalho Remota no console do Gerenciador do Servidor <kbd>(Adicionar Funções e Recursos -> Serviços de Área de Trabalho Remota -> Licenciamento de Área de Trabalho Remota ).</kbd>


## Ative o Servidor de Licenças dos Serviços de Área de Trabalho Remota

Para emitir licenças para clientes RDP, seu Servidor de Licença RDS deve ser ativado. Abra o **Remote Desktop Licensing Manager** <kbd>(licmgr.exe)</kbd>, clique com o botão direito do mouse no nome do seu servidor e selecione Ativar Servidor .

Imagem aqui

No assistente de ativação do servidor de licenciamento RDS, escolha se deseja ativar o servidor pela Internet, usando um navegador ou por telefone.

Imagem aqui

Em seguida, preencha algumas informações sobre sua empresa (alguns campos são obrigatórios).

Imagem aqui

Clique no botão Finalizar . A seguinte mensagem deve aparecer:
