---
title: Como inserir um papel de parede via GPO
description: Como inserir um papel de parede via GPO
author: soarespaullo
date: 2024-12-31 18:50:00
categories: [Windows, SysAdmin]
tags: [Tutoriais, Windows, Servidor, SysAdmin, GPO, Wallpaper]
math: true
mermaid: true
image:
  path: /assets/img/img-blog/gpo/gpo.png
  alt: Como inserir um papel de parede via GPO

---

## Definir papel de parede de fundo da Área de Trabalho usando a Política de Grupo

>As Políticas de Grupo permitem que você defina o mesmo `papel de parede da Área de Trabalho` em todos os computadores do Domínio.
{: .prompt-info }

Primeiramente, vamos criar uma pasta no servidor. Neste caso, criei uma pasta chamada **wallpapers** dentro do (C:). Dentro dessa pasta, insira o papel de parede que será utilizado.

![Desktop View](/assets/img/img-blog/gpo/pasta.png)

Em seguida, clique com o botão direito do mouse na pasta, procure pela aba **compartilhamento e compartilhe-a**. No exemplo abaixo, notamos que ela está compartilhada pelo caminho: <kbd>\\\vmserver\wallpapers$</kbd>. 

>O símbolo $ `dólar` ao final do nome da pasta, indica que o compartilhamento ficará oculto e não aparecerá em meus `Locais de Rede`.
{: .prompt-info }

![Desktop View](/assets/img/img-blog/gpo/caminho.png)

Agora clique na aba **Segurança** editar e adicione o grupo **usuários do domínio** e dê as permissões de **Ler** & **Executar**, **Listar conteúdo da pasta** e **Leitura**. Defina as permissões, clique em Aplicar e depois OK.

![Desktop View](/assets/img/img-blog/gpo/permissao.png)

>Essas permissões são para que os usuários possam `ler` a `foto/papel de parede` a ser aplicada e consigam receber a configuração.
{: .prompt-info }

Agora abra o console de gerenciamento de GPO de Domínio <kbd>gpmc.msc</kbd>
Crie uma nova política de grupo, <kbd>GPO - Wallpaper</kbd>, e atribua-a à **UO - Unidade Organizacional** com os usuários aos quais você deseja aplicar o papel de parede. Edite o GPO que você criou;

![Desktop View](/assets/img/img-blog/gpo/editar.png)

Vá para **Configuração do Usuário -> Políticas -> Modelos administrativos -> Área de Trabalho -> Active Desktop -> Papel de parede da Área de Trabalho**

![Desktop View](/assets/img/img-blog/gpo/papel.png)

Em seguida, habilite e configure a política **Papel de parede da Área de Trabalho**. Especifique o caminho **UNC (Universal Naming Convention)** para o arquivo de imagem **BMP/JPEG**.

![Desktop View](/assets/img/img-blog/gpo/habilitar.png)

>O estilo de papel de parede `Preencher` aumenta ou diminui a imagem para se ajustar à largura da tela. Ele fica bem em quase qualquer resolução de tela.
{: .prompt-info }

## Impedindo a alteração do plano de fundo da Área de Trabalho

Se você quiser impedir que os usuários alterem a imagem do papel de parede, ative a política **Proibir alterações** em **Configuração do Usuário -> Políticas -> Modelos administrativos -> Área de Trabalho/Active Desktop**

![Desktop View](/assets/img/img-blog/gpo/proibir.png)

## Verificando se o Usuário consegue alterar o papel de parede

Agora logue em alguma estação e verifique se os usuários estão conseguindo alterar o papel de parede da Área de Trabalho.

![Desktop View](/assets/img/img-blog/gpo/usuario.png)

>Se a Política de Grupo não for aplicada aos computadores dos usuários, execute o `diagnóstico` em um cliente específico usando o comando `gpresult /r`.
{: .prompt-warning }

## Testando se o papel de parede foi aplicado corretamente via GPO

Verifique se a nova imagem de papel de parede é exibida na Área de Trabalho.

![Desktop View](/assets/img/img-blog/gpo/teste.png)

## Algumas observações caso não funcione a aplicação do papel de parede via GPO

- Verifique se o compartilhamento está OK antes de aplicar a diretiva;
- Verifique se o(s) usuário(s) estão na(s) OU(s) abaixo(s) da GPO criada;
- Verifique se há rede na estação;
- Verifique se o(s) grupo(s) usuários autenticados/dominio/domain users está na na aba Segurança da pasta compartilhada e se o(s) mesmo(s) estão com permissões de leitura.

>A `GPO` demora em média `20 minutos` para ser aplicada, e irá atualizar na estação de trabalho após reinicialização do sistema operacional.
{: .prompt-warning }

**Como inserir um papel de parede via GPO**. PROFISSIONAISTI. Disponível em: [**profissionaisti.com.br**](https://www.profissionaisti.com.br/wallpaper-papel-parede-gpo/){:target="_blank"}. Acesso em: 31 de dez. de 2024.

**Set Desktop Wallpaper and Logon Screen Background via Group Policy**. WOSHUB. Disponível em: [**woshub.com**](https://woshub.com/setting-desktop-wallpapers-background-using-group-policy/){:target="_blank"}. Acesso em: 31 de dez. de 2024.
