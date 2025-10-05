---
title: Ocultar unidades específicas no Explorador de Arquivos via GPO
description: Como ocultar uma partição de disco (unidade) no Windows
author: soarespaullo
date: 2025-10-05 15:45:00
categories: [Windows, SysAdmin]
tags: [Tutoriais, Windows, Servidor, SysAdmin]
math: true
mermaid: true
image:
  path: /assets/img/img-blog/windows-server/unidade-disco.png
  alt: Como ocultar uma partição de disco (unidade) no Windows

---

No Windows, um administrador pode ocultar uma unidade específica (partição de disco) no Explorador de Arquivos e impedir que outros usuários a acessem.

1. No servidor, pressione <kbd>Win + R</kbd>, digite <kbd>gpmc.msc</kbd> e pressione Enter.

2. No painel esquerdo, expanda seu domínio. Clique com o botão direito na `(OU - Unidade Organizacional)` desejada (ex: ts) e selecione `"Criar um GPO neste domínio e fornecer um link para ele aqui…"`

3. Nomeie a GPO `ex: GPO - Ocultar Unidade D:)`

4. Clique com o botão direito na GPO criada e `Editar`

5. Navegue até `Configuração do Usuário -> Modelos Administrativos -> Componentes do Windows -> Explorador de Arquivos
Abra as configurações da opção Ocultar estas unidades especificadas em Meu Computador`.

Com esta opção de GPO, você pode ocultar unidades específicas (A, B, C, D) ou todas as unidades de uma só vez. Por exemplo, escolhi ocultar apenas a unidade D:.


> Mas esta GPO não permite que você especifique manualmente outras letras de unidade para ocultar.
{: .prompt-warning }

![Desktop View](/assets/img/img-blog/windows-server/ocultar.png)

Salve as alterações. As configurações da Política de Grupo serão aplicadas imediatamente, sem a necessidade de reinicialização. A unidade D: ficará oculta da navegação no Explorador de Arquivos.

![Desktop View](/assets/img/img-blog/windows-server/disco.png)

No entanto, os usuários ainda podem abrir este disco digitando seu endereço `(letra da unidade)` na barra de endereços do Explorer manualmente.

![Desktop View](/assets/img/img-blog/windows-server/explorer.png)

Na mesma seção GPO, há outra opção: `Impedir acesso a unidades de Meu Computador. Habilitar esta opção de GPO e escolher uma unidade específica nas configurações impedirá que os usuários acessem o disco (visualizando seu conteúdo no Explorador de Arquivos)`.

![Desktop View](/assets/img/img-blog/windows-server/restringir.png)

Se um usuário tentar abrir qualquer caminho em uma unidade oculta, um erro aparecerá:

![Desktop View](/assets/img/img-blog/windows-server/nao-permitido.png)

> Esta operação foi cancelada devido a restrições em vigor neste computador. Entre em contato com o administrador do sistema.
{: .prompt-warning }

> No entanto, unidades ocultas dessa forma ainda estarão visíveis em aplicativos de terceiros, como gerenciadores de arquivos, arquivadores, etc. As unidades ficam ocultas apenas na interface gráfica do usuário do Explorador de Arquivos.
{: .prompt-info }
