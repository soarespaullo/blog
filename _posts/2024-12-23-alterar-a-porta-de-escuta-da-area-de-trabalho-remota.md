---
title: Alterar a porta 3389 da Área de Trabalho Remota padrão (RDP) no Windows
description: Alterar a porta de escuta da Área de Trabalho Remota em seu computador
author: soarespaullo
date: 2024-12-22 17:50:00
categories: [Windows, SysAdmin]
tags: [Tutoriais, Windows,Servidor, SysAdmin, RDP, Port]
math: true
mermaid: true
image:
  path: /assets/img/img-blog/windows-server.png
  alt: Alterar a porta de escuta da Área de Trabalho Remota em seu computador

---

Em todos os sistemas operacionais Windows, a porta padrão atribuída ao RDP **(Remote Desktop Protocol)** é TCP 3389.
Após habilitar o RDP no Windows, o <kbd>TermService</kbd> **(Remote Desktop Services)** começa a escutar na porta **3389**. Neste artigo, 
mostraremos como alterar o número da porta RDP padrão nas edições desktop do Windows (10/11) e Windows Server usando o Editor do Registro do Windows. 

> Ao escolher uma porta RDP não padrão, observe que não é recomendado usar portas no intervalo <kbd>1-1023</kbd> (portas conhecidas). Use uma porta dinâmica no intervalo de portas RPC <kbd>(49152 to 65535)</kbd> , 
ou qualquer porta no intervalo <kbd>1024 a 49151</kbd> que não esteja em uso por outro serviço ou aplicativo.
{: .prompt-warning }
>

## Como alterar a porta da área de trabalho remota no Windows?

Em nosso exemplo, alteraremos o número da porta na qual o serviço **Remote Desktop** está escutando **10050** . Para fazer isso:

1. Abra o Editor do Registro <kbd>regedit.exe</kbd> e vá até a chave de registro **HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp** ;
2. Encontre o parâmetro **DWORD** com o nome **PortNumber** . Este parâmetro mostra a porta na qual o serviço Remote Desktop está escutando. O padrão é 3389 (decimal);
3. Altere o valor deste parâmetro. Eu alterei a porta RDP para **10050** (Decimal)

