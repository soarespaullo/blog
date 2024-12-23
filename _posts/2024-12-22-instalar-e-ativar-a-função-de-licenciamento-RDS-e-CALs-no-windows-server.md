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

> Este servidor de licença é um membro do grupo Terminal Server License Servers no Active Directory. Este servidor de licença poderá emitir RDS Per User CALs para usuários no domínio, e você poderá rastrear o uso de RDS Per User CALs.
{: .prompt-info }

> Este servidor de licença é registrado como um ponto de conexão de serviço (SCP) nos Serviços de Domínio do Active Directory.
{: .prompt-info }

![Desktop View](/assets/img/img-blog/pronto.png)

## Instalar licenças de acesso de cliente (CALs) do RDS no Windows Server

Agora você precisa instalar o pacote de licença de acesso de cliente de terminal (RDS CAL) que você comprou no servidor de licenças. Existem dois tipos de RDS CALs:

- **CAL por dispositivo** – é a licença permanente atribuída a um computador (dispositivo) que se conecta ao servidor RDS mais de uma vez (um licenciado temporário é emitido quando um dispositivo é conectado pela primeira vez). Essas licenças não são simultâneas, então se você tiver 10 licenças por dispositivo, apenas 10 computadores poderão se conectar ao seu host RDS.

- **CAL por usuário** – a licença permite que um usuário se conecte ao RDS de qualquer número de computadores/dispositivos. Este tipo de licença é vinculado a uma conta de usuário no Active Directory. É emitido por um período de 52 a 89 dias (número aleatório).

> Se você estiver implantando um HOST RD em um grupo de trabalho **(sem um domínio AD)** , use CALs RDS por dispositivo. Caso contrário, o servidor RDSH encerrará à força a sessão do usuário a cada 60 minutos:
Problema com a licença da Área de Trabalho Remota: Há um problema com sua licença da Área de Trabalho Remota e sua sessão será desconectada em 60 minutos
{: .prompt-warning }

As RDS CALs que você usa devem ser compatíveis com a versão do Windows Server à qual os usuários ou dispositivos estão se conectando. Para determinar a compatibilidade da RDS CAL com versões do Windows Server no servidor de licença RD, consulte a tabela a seguir:

|              | CAL R2 2008  |   CAL 2012   |   CAL 2016   |   CAL 2019   |   CAL 2022   |
|  ----------- | ------------ | ----------   | ------------ | ------------ | ------------ |
|  2008 R2     |     Sim      |     Não      |      Não     |     Não      |     Não      |
|  2012        |     Sim      |     Sim      |      Não     |     Não      |     Não      |
|  2012 R2     |     Sim      |     Sim      |      Não     |     Não      |     Não      |
|  2016        |     Sim      |     Sim      |      Sim     |     Não      |     Não      |
|  2019        |     Sim      |     Sim      |      Sim     |     Sim      |     Não      | 
|  2022        |     Sim      |     Sim      |      Sim     |     Sim      |     Sim      |


> RDS CALs para novas versões do Windows Server não podem ser instaladas em versões anteriores do WS. Por exemplo, você não poderá instalar as RDS CALs de 2022 em um host de licenciamento do Windows Server 2016.
{: .prompt-danger }

Clique com o botão direito do mouse no seu host no console do Gerenciador de Licenciamento de Área de Trabalho Remota e selecione Instalar Licenças .

![Desktop View](/assets/img/img-blog/instalar.png)

Selecione o método de ativação (automático, online ou por telefone) e o programa de licença (no nosso caso, é o Contrato de Empresa).

> Vários números de acordos empresariais do RDS vazaram na internet <kbd>4965437</kbd>, você nem precisa procurar por cracks ou ativadores do RDS.
{: .prompt-warning }

![Desktop View](/assets/img/img-blog/empresa.png)

As próximas etapas do assistente dependem de qual programa de licença você selecionou. No caso de um Enterprise Agreement, você deve fornecer seu número. Se você escolheu License Pack (Retail Purchase), insira a chave de produto de 25 caracteres que você recebeu da Microsoft ou de um parceiro.

![Desktop View](/assets/img/img-blog/licenca.png)

Especifique a versão do produto **(Windows Server 2022, 2019 ou 2016)**, o tipo de RDS CAL e o número de licenças de terminal a serem instaladas no servidor.

![Desktop View](/assets/img/img-blog/cal.png)

Você pode converter CALs de usuário do RDS em CALs de dispositivo (e vice-versa) usando a opção **Converter licenças** no console do Gerenciador de Licenciamento.

![Desktop View](/assets/img/img-blog/converter.png)

## Configurar definições de licenciamento em hosts de sessão RD

Vá para Configuração do Computador -> Políticas -> Modelos de Administração -> Componentes do Windows -> Serviços de Área de Trabalho Remota -> Host de Sessão de Área de Trabalho Remota -> Licenciamento e configure as seguintes opções:

- **Use os servidores de licença de Área de Trabalho Remota especificados** – especifique o nome ou o endereço IP do servidor onde a licença RDS está instalada;
- **Defina o modo de licenciamento da Área de Trabalho Remota** – selecione o tipo de licença para RDS CALs.

![Desktop View](/assets/img/img-blog/gpedit.png)

> Se você instalou o RDSH na edição Windows Server Evaluation , você deve convertê-lo para a versão completa. Sem uma atualização, os serviços RDSH em tal host funcionarão apenas por 120 dias, mesmo se você direcioná-lo para um servidor de licença RDS ativado.
{: .prompt-danger }

Execute o Diagnosticador de licenciamento de área de trabalho remota <kbd>lsdiag.msc</kbd> no RDSH e verifique se ele vê o servidor de licenciamento e o número de RDS CALs disponíveis.

Se não houver avisos e você vir a mensagem, o servidor RDSH poderá receber CALs RDS com sucesso para usuários e/ou dispositivos remotos.

> O Diagnóstico de Licenciamento não identificou nenhum problema de licenciamento para o servidor Host de Sessão da Área de Trabalho Remota.
{: .prompt-info }

![Desktop View](/assets/img/img-blog/diag.png)

Pronto, finalizamos a instalação e configuração das licenças CALs de RDS no Windows Server..., até breve! =D
