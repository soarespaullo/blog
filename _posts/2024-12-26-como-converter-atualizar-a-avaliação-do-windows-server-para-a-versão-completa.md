---
title: Como converter (atualizar) a avaliação do Windows Server para a versão completa
description: Como converter (atualizar) a avaliação do Windows Server para a versão completa
author: soarespaullo
date: 2024-12-26 21:50:00
categories: [Windows, SysAdmin]
tags: [Tutoriais, Windows, Servidor, SysAdmin, DISM]
math: true
mermaid: true
image:
  path: /assets/img/img-blog/winserver/winserver.png
  alt: Como converter (atualizar) a avaliação do Windows Server para a versão completa

---

A Microsoft convida todos a explorar os recursos da plataforma Windows Server com as edições de teste **Standard Evaluation** ou **Datacenter Evaluation**. Você pode baixar uma imagem **ISO** ou **VHD** com a versão gratuita do <kbd>Windows Server 2022, 2019, 2016 ou 2012 R2</kbd> no site do [**Microsoft Evaluation Center preenchendo um pequeno formulário**](https://www.microsoft.com/en-us/evalcenter). Esta edição do Windows Server é apenas para fins de avaliação, teste ou treinamento e não é para uso comercial. Depois de instalar o Windows Server Evaluation, você tem **180 dias** para testar seus recursos.


>Suponha que você esteja executando tarefas de produção no Windows Server Evaluation. Nesse caso, há uma maneira de convertê-lo para uma edição completa do Windows Server, mantendo seus dados e sem reinstalar o sistema operacional.
{: .prompt-info }

## Como estender o período de avaliação gratuita do Windows Server

Se você estiver usando a versão de avaliação do Windows Server, a edição atual e o tempo restante antes do período de carência expirar serão exibidos na área de trabalho <kbd>(Licença do Windows válida para 176 dias).</kbd>

![Desktop View](/assets/img/img-blog/winserver/licenca.png){: width="972" }
_Windows Server_

Você também pode obter o período de carência restante do Windows Server com o comando <kbd>slmgr /dli</kbd>

![Desktop View](/assets/img/img-blog/winserver/slmgr.png){: width="972" }
_Windows Server_

>O período de teste gratuito do Windows Server pode ser legalmente estendido <kbd>5 vezes por 180 dias</kbd> usando o comando <kbd>slmgr /rearm</kbd>
{: .prompt-info }

Isso significa que o tempo máximo de uso do Windows Server avaliado é de <kbd>3 anos (180 dias * 6).</kbd>

Após o término do período de teste, o Windows Server começa a exigir ativação e desliga a cada hora com as seguintes mensagens nos logs do Visualizador de Eventos

>O período de licença para esta instalação do Windows expirou. O sistema operacional será desligado a cada hora.
{: .prompt-warning }


## Atualizar a avaliação do Windows Server para uma versão de varejo completa

A Microsoft permite que o Windows Server Evaluation seja atualizado para uma edição completa de varejo **Standard** ou **Datacenter**.

Aqui estão algumas restrições de atualização:

- Você pode atualizar para a mesma edição ou uma edição superior do Windows Server. Por exemplo, **Server Standard Eval** pode ser atualizado para **Server Standard** ou **Server Datacenter**;

- Não é recomendado **converter um host com a função de controlador de domínio do AD**. Você deve primeiro **rebaixar o DC para um servidor membro**. Isso requer que o domínio tenha pelo menos um **DC adicional para o qual você pode mover funções FSMO**;

- Se o **NIC Teaming** estiver configurado no servidor, ele deverá ser desabilitado antes da atualização.

O primeiro passo é garantir que você tenha uma versão de avaliação do Windows Server instalada:

```text
DISM /online /Get-CurrentEdition
```

![Desktop View](/assets/img/img-blog/winserver/dism.png)

Liste as edições do Windows Server para as quais você está qualificado para atualizar sua cópia de avaliação:

```
DISM /online /Get-TargetEditions
```

>Edições que podem ser atualizadas: <kbd>ServerStandard e ServerDatacenter.</kbd>
{: .prompt-info }

![Desktop View](/assets/img/img-blog/winserver/edicao.png)

Isso significa que você pode atualizar seu **Server Standard Eval** para **Server Datacenter** ou **Server Standard**.

Use o comando do <kbd>PowerShell</kbd> para verificar sua versão do Windows Server

```
Get-ComputerInfo | select OsName, WindowsEditionId
```

![Desktop View](/assets/img/img-blog/winserver/powershell.png)

>Você pode converter tanto a versão GUI completa do Windows Server quanto o Windows Server Core.
{: .prompt-info }

Para converter uma avaliação do **Windows Server** em uma versão completa, você precisará usar a **chave de configuração do cliente KMS pública** (ou Chave de Licença de Volume Genérica, **GVLK**) para sua versão do **Windows Server**. Para obter uma lista completa de chaves **GVLK** para todas as versões do Windows, visite o site da Microsoft [**Key Management Services (KMS) client activation and product keys**](https://learn.microsoft.com/en-us/windows-server/get-started/kms-client-activation-keys?tabs=server2025%2Cwindows1110ltsc%2Cversion1803%2Cwindows81). A lista completa de chaves para todas as versões suportadas do Windows Server pode ser encontrada na tabela abaixo:




|  Versão/edição do SO      |            STANDARD           |          DATACENTER           |
| ------------------------- | ----------------------------- | ----------------------------- |
|  Servidor Windows 2022    | VDYBN-27WPP-V4HQT-9VMD4-VMK7H | WX4NM-KYWYW-QJJR4-XV3QB-6VM33 |    
|  Servidor Windows 2019    | N69G4-B89J2-4G8F4-WWYCC-J464C | WMDGN-G9PQG-XVVXX-R3X43-63DFG |
|  Servidor Windows 2016    | WC2BQ-8NRM3-FDDYY-2BFGV-KHKQY | CB7KF-BWN84-R7R2Y-793K2-8XDDG |  
|  Servidor Windows 2012 R2 | D2N9P-3P6X9-2R39C-7RTCD-MDVJX | KNC87-3J2TX-XB4WP-VCPJV-M4FWM |

Neste caso, vamos atualizar a edição Eval para a versão de varejo completa do **Windows Server 2019 Standard**. Encontre e copie a chave **GVLK** para a versão e edição de destino do **Windows Server** da tabela e, em seguida, execute o seguinte comando:

```
dism /online /set-edition:ServerStandard /productkey:xxxxx-xxxxx-xxxxx-xxxxx-xxxxx /accepteula
```

![Desktop View](/assets/img/img-blog/winserver/converte.png)

>Se você precisar alterar o sistema operacional para a edição **Datacenter**, use o <kbd>/set-edition:ServerDatacenter</kbd> e o **GVLK** apropriado da tabela.
{: .prompt-info }

Reinicie o computador e verifique se sua cópia de avaliação do Windows Server foi convertida para uma edição de varejo completa.


**How to Convert (Upgrade) Windows Server Evaluation to Full Version**. WOSHUB. Disponível em: [**woshub.com**](https://woshub.com/how-to-upgrade-windows-server-2016-evaluation-to-full-version/){:target="_blank"}. Acesso em: 26 de dez. de 2024.
