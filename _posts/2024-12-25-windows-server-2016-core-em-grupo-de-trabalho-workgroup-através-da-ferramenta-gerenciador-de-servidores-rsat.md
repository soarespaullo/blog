---
title: Administrar Windows Server 2016 Core em Grupo de Trabalho (Workgroup) através da ferramenta Gerenciador de Servidores (RSAT)
description: Fazer backup de máquinas virtuais Hyper-V
author: soarespaullo
date: 2024-12-24 17:00:00
categories: [Windows, SysAdmin]
tags: [Tutoriais, Windows, Servidor, SysAdmin, RSAT]
math: true
mermaid: true
image:
  path: /assets/img/img-blog/rsat/rsat.png
  alt: Administrar Windows Server 2016 Core em Grupo de Trabalho (Workgroup) através da ferramenta Gerenciador de Servidores (RSAT)

---


Em ambientes de rede baseados em estações e servidores Microsoft Windows o recomendado é que tenhamos os hosts fazendo parte de um domínio **(Active Directory)**, 
porém nos deparamos com algumas situações em que todos ou alguns hosts não fazem ou não podem fazer parte de um domínio.

A partir do Windows Server 2008 a ferramenta Gerenciador de Servidores **(Server Manager)**
foi incorporada nos servidores para que possamos de um ponto central gerenciar todos os servidores da rede.

Em redes onde os hosts fazem parte de um domínio, a tarefa de gerenciar servidores remotamente através do **Gerenciador de Servidores** é fácil de ser realizada,
porém quando os hosts fazem parte de um grupo de trabalho **(workgroup)**, para que possamos gerenciar servidores remotamente é necessário que realizemos alguns passos.

O mais comum é gerenciarmos servidores remotamente através de um servidor, porém nesse artigo vamos gerenciar um servidor através de uma estação de trabalho.

Esse artigo pode ajudar também aos colegas que tem em seu ambiente servidores com a instalação Server Core **(sem interface gráfica)**, facilitando assim a administração desses servidores.
Os passos são os mesmos tanto para servidor com interface gráfica quanto para server core.

> Esse procedimento foi realizado utilizando uma estação cliente Windows 10 Versão 22H2 (Compilação do Sistema Operacional 19045.5247) com o Gerenciador do Servidor (Compilação 10.0.19041.1)
e Servidor Windows Server 2016 Standand Evaluation Versão 1809 (Compilação do Sistema Operacional 17763.3650).
Com o ambiente de rede em grupo de trabalho (Workgroup) e com o hostname do servidor publicado no DNS da rede.
{: .prompt-warning }

> O hostname do servidor que será gerenciado deve estar mapeado no arquivo **“hosts”** <kbd>(C:\Windows\System32\drivers\etc)</kbd> na máquina cliente ou publicado no servidor DNS da rede.
{: .prompt-warning }

Primeiramente na máquina cliente Windows 10 devemos instalar o pacote **RSAT** <kbd>(Ferramenta de Administração de Servidor Remoto)</kbd> para que depois possamos utilizar a ferramenta Gerenciador de Servidores.
Devemos baixar a ferramenta **RSAT** no seguinte link: [Ferramentas de Administração de Servidor Remoto para Windows 10](https://www.microsoft.com/pt-BR/download/details.aspx?id=45520).
Após baixarmos a ferramenta devemos instalar.

