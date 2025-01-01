---
title: Alterar a porta 3389 da Área de Trabalho Remota padrão (RDP) no Windows
description: Alterar a porta de escuta da Área de Trabalho Remota em seu computador
author: soarespaullo
date: 2024-12-22 17:50:00
categories: [Windows, SysAdmin]
tags: [Tutoriais, Windows, Servidor, SysAdmin, RDP, Porta]
math: true
mermaid: true
image:
  path: /assets/img/img-blog/remote-desktop/remote-desktop.png
  alt: Alterar a porta de escuta da Área de Trabalho Remota em seu computador

---

Em todos os sistemas operacionais Windows, a porta padrão atribuída ao RDP **(Remote Desktop Protocol)** é **TCP 3389**.
Após habilitar o RDP no Windows, o <kbd>TermService</kbd> **(Remote Desktop Services)** começa a escutar na porta **3389**. Neste artigo, 
mostraremos como alterar o número da porta RDP padrão nas edições desktop do Windows (10/11) e Windows Server usando o Editor do Registro do Windows. 

> Ao escolher uma porta RDP não padrão, observe que não é recomendado usar portas no intervalo <kbd>1-1023</kbd> (portas conhecidas). Use uma porta dinâmica no intervalo de portas RPC <kbd>(49152-65535)</kbd> , 
ou qualquer porta no intervalo <kbd>1024 a 49151</kbd> que não esteja em uso por outro serviço ou aplicativo.
{: .prompt-warning }


## Como alterar a porta da Área de Trabalho Remota no Windows?

Em nosso exemplo, alteraremos o número da porta na qual o serviço **Remote Desktop** está escutando para **10050**.

1. Abra o Editor do Registro <kbd>Win+R</kbd> e escreva <kbd>regedit.exe</kbd> e vá até a chave de registro **HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp**;
2. Encontre o parâmetro **DWORD** com o nome **PortNumber**. Este parâmetro mostra a porta na qual o serviço Remote Desktop está escutando. O padrão é 3389 (decimal);
3. Altere o valor deste parâmetro. Eu alterei a porta RDP para **10050** (Decimal)

![Desktop View](/assets/img/img-blog/remote-desktop/port.png) 

> Alterar a porta `RDP` reduzirá as chances de explorar vulnerabilidades `RDP` (a última vulnerabilidade crítica no `(RDP - BlueKeep)` é descrita em `CVE-2019-0708`, reduzirá o número de ataques de força bruta RDP `(não se esqueça de analisar regularmente os logs de conexão RDP)`, `SYN` e outros tipos de ataques quando o `NLA` estiver desabilitado . Na maioria das vezes, a porta `RDP` é alterada em computadores com conexão direta à Internet `(VPS/ VDS)` ou em redes onde o roteador de borda encaminha a porta `3389/RDP` para um host Windows em sua `LAN`.
{: .prompt-warning }

## Atualizando Regras do Firewall do Windows

Se o Firewall do Windows estiver habilitado no seu computador, você terá que criar uma nova regra que permita conexão de entrada para sua nova porta RDP

> Se você estiver reconfigurando um host remoto do Windows via `Remote Desktop Protocol`, certifique-se de criar regras de permissão no firewall antes de reiniciar o `TermService`, caso contrário, você perderá o acesso ao servidor
{: .prompt-warning }

Você pode criar uma regra de permissão de entrada para sua nova porta **TCP/UDP** **(Remote Desktop Protocol)** manualmente no console do Firewall do Windows.
Execute as combinações de teclas <kbd>Win+R</kbd> e escreva <kbd>firewall.cpl</kbd>

- Selecione **Configurações Avançadas**.
- Selecione **Regras de Entrada**.
- No painel **Ações**, selecione **Nova Regra**.
- Escolha o **tipo de regra de Porta** e selecione Avançar.
- Na página **Protocolo e Portas**, escolha **TCP**.
- Selecione **Portas Locais Específicas**, insira o valor **10050** e clique em Avançar.
- Na página de **Ação**, selecione **Permitir a conexão** e clique em Avançar.
- Na página de **Perfil**, **selecione as opções apropriadas para o seu ambiente** e clique em Avançar.
- Na página de **Nome**, insira o **nome da regra**, no meu caso, vou colocar **(TS 10050)** e selecione Concluir.

Após concluir a configuração do Firewall, reinicie o computador <kbd>shutdown -f -t 0 -r</kbd> ou reinicie o serviço de **Área de Trabalho Remota** <kbd>net stop termservice & net start termservice</kbd>

![Desktop View](/assets/img/img-blog/remote-desktop/restart.png)

Para se conectar a este host Windows via **Área de Trabalho Remota**, você precisa especificar a nova porta de conexão **RDP** no seu cliente <kbd>mstsc.exe</kbd> usando dois pontos da seguinte forma: **localhost:10050** ou pelo endereço **IP: 127.0.0.1:10050** ou no prompt de comando: **mstsc.exe /v 127.0.0.1:10050**

![Desktop View](/assets/img/img-blog/remote-desktop/mstsc.png)

Então você se conectará com sucesso à **Área de Trabalho Remota** de um computador usando a nova porta **RDP**. Você pode usar o comando <kbd>netstat –na | Find “LIST”</kbd> para certificar-se de que seu **Remote Desktop Service** esteja escutando em uma nova porta.

![Desktop View](/assets/img/img-blog/remote-desktop/netstat.png)

Este guia para alterar a porta **RDP** padrão é adequado para qualquer versão do Windows, desde o **Windows XP** **(Windows Server 2003)** até as compilações modernas do **Windows 10**, **Windows 11** e **Windows Server 2022**.

**Change the Default Remote Desktop (RDP) Port 3389 in Windows**. WOSHUB. Disponível em: [**woshub.com**](https://woshub.com/change-rdp-port-3389-windows/){:target="_blank"}. Acesso em: 22 de dez. de 2024.
