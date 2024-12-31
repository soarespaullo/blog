---
title: Configurando limites de sessões RDP/RDS (tempos limite) no Windows
description: Configurando limites de sessões RDP/RDS (tempos limite) no Windows
author: soarespaullo
date: 2024-12-29 16:50:00
categories: [Windows, SysAdmin]
tags: [Tutoriais, Windows, Servidor, SysAdmin, RDS, RDP]
math: true
mermaid: true
image:
  path: /assets/img/img-blog/rdp-rds/rds.png 
  alt: Configurando limites de sessões RDP/RDS (tempos limite) no Windows

---

Quando um usuário fecha a janela de sessão **RDP/RDS** em um cliente de terminal <kbd>mstsc.exe, RDCMan ou cliente web HTML5 do Remote Desktop</kbd> simplesmente clicando na cruz **(X)** no canto superior direito sem fazer logoff,
sua sessão vai do modo **ativo** para o modo **desconectado**. Neste modo, todos os aplicativos, documentos abertos e janelas ainda estão em execução em um computador do **Remote Desktop** e consumindo recursos do sistema.

Por padrão, uma sessão **RDP** de um usuário no Windows pode permanecer no estado desconectado até ser encerrada pelo usuário ou administrador, ou até que o computador seja reiniciado. É bem conveniente,
já que um usuário pode a qualquer momento se conectar à sua sessão de área de trabalho remota anterior e continuar trabalhando com aplicativos em execução e arquivos abertos.

>A captura de tela a seguir mostra que sessões de usuário desconectadas em um servidor `RDS` executando o Windows Server 2019 consomem cerca de 20% da RAM do servidor.
{: .prompt-warning }

![Desktop View](/assets/img/img-blog/rdp-rds/ram.png)

Além disso, essas sessões podem bloquear arquivos abertos em seus servidores de arquivos, causar problemas com salvamento incorreto de dados nos aplicativos, pastas de perfil de roaming ou em Discos de Perfil de Usuário.
Sessões RDP desconectadas geralmente causam um problema de bloqueio de conta de usuário de domínio após uma alteração de senha <kbd>quando a sessão RDS continua a ser executada sob a senha antiga do usuário.</kbd>

Usando o comando <kbd>quser</kbd>, você pode ver quando uma sessão **RDP** do usuário foi iniciada, quanto tempo ela ficou ociosa e o estado atual da sessão.

![Desktop View](/assets/img/img-blog/rdp-rds/quser.png)

No **Windows Server 2022/2019/2016/2012R2**, você pode definir tempos limite de sessão **RDP** usando **Políticas de Grupo**. Você pode fazer isso no editor de **GPO de Domínio** <kbd>gpmc.msc</kbd> ou no Editor de **Política de Grupo Local** <kbd>gpedit.msc</kbd>
em um host RDS específico <kbd>ou em uma versão desktop do Windows se você tiver permitido várias conexões RDP a ele.</kbd>

As configurações de tempo limite de sessão RDP estão localizadas na seguinte seção GPO **Configuração do Computador -> Políticas -> Modelos Administrativos -> Componentes do Windows -> Serviços de Área de Trabalho Remota -> Host de Sessão da Área de Trabalho Remota -> Limites de Tempo de Sessão**.
As seguintes configurações de tempo limite de Área de Trabalho Remota estão disponíveis:

- **Definir limite de tempo para sessão desconectada;**
- **Definir limite de tempo para sessões ativas, mas ociosas, dos Serviços de Área de Trabalho Remota** — a política permite encerrar sessões RDP ociosas que não têm nenhuma entrada do usuário (como mover um mouse ou digitar algo em um teclado);
- **Definir limite de tempo para sessões ativas dos Serviços de Área de Trabalho Remota** — é o tempo máximo de qualquer sessão RDP (mesmo uma ativa), após o qual ela muda para o estado desconectado;
- **Encerrar sessão quando os limites de tempo forem atingidos** — define o tempo após o qual uma sessão RDP será encerrada (logoff) em vez de ser desconectada;
- **Defina um limite de tempo para logoff de sessões do RemoteApp**.

![Desktop View](/assets/img/img-blog/rdp-rds/tempo.png)

Por padrão, essas opções não são configuradas. Para encerrar automaticamente todas as sessões de usuário **RDP** desconectadas em 1 horas, 
habilite as políticas **(Definir limite de tempo para sessões desconectadas)** e **(Definir limite de tempo para sessões ativas, mas ociosas, dos Serviços de Área de Trabalho Remota)** e selecione 1 horas na lista suspensa.

![Desktop View](/assets/img/img-blog/rdp-rds/hora.png)

Salve as alterações e atualize as configurações da **Política de Grupo** no seu host RD <kbd>gpupdate /force</kbd>. As novas configurações de tempo limite serão aplicadas somente a novas sessões **RDP**
<kbd>você terá que encerrar as sessões de usuário atuais no RDSH manualmente</kbd>.

Você também pode definir os limites de uma sessão **RDP** na guia Configurações nas propriedades de um <kbd>lusrmgr.msc</kbd> usuário local ou de domínio <kbd>dsa.msc</kbd> (Usuários e Computadores do Active Directory). As seguintes opções estão disponíveis aqui;

- Encerrar uma sessão desconectada;
- Limite de sessões ativas;
- Limite de sessão ociosa;
- Quando um limite de sessão é atingido ou a conexão é interrompida: “Desconectar-se da sessão” ou “Encerrar sessão”;
- Permitir reconexão: “De qualquer cliente” ou “Apenas do cliente originador”.

![Desktop View](/assets/img/img-blog/rdp-rds/dsa.png)

>Você não deve tornar os tempos limite das sessões RDP muito curtos, caso contrário, as sessões dos usuários terminarão quase imediatamente após ficarem inativas.
{: .prompt-warning }

Você pode permitir várias conexões na mesma conta de usuário para o host **RDP** usando a opção <kbd>GPO Restringir usuários dos Serviços de Área de Trabalho Remota a uma única sessão dos Serviços de Área de Trabalho Remota</kbd> = <kbd>Desativado</kbd>
em **(Configuração do Computador -> Modelos Administrativos -> Componentes do Windows -> Serviços de Área de Trabalho Remota -> Host da Sessão da Área de Trabalho Remota -> Conexões)**.

![Desktop View](/assets/img/img-blog/rdp-rds/user.png)

**Configuring RDP/RDS Sessions Limits (Timeouts) on Windows**. WOSHUB. Disponível em: [**woshub.com**](https://woshub.com/remote-desktop-session-time-limit/){:target="_blank"}. Acesso em: 29 de dez. de 2024.
