---
title: Como fazer backup completo de VM do Hyper-V
description: Fazer backup de máquinas virtuais Hyper-V
author: soarespaullo
date: 2024-12-24 17:00:00
categories: [Windows, SysAdmin, Hyper-V, VM]
tags: [Tutoriais, Windows, Servidor, SysAdmin, Hyper-V, VM]
math: true
mermaid: true
image:
  path: /assets/img/img-blog/hyper-v/hyper-v.png
  alt: Fazer backup de máquinas virtuais Hyper-V

---

   
Neste artigo, vou demonstrar de forma simples como fazer backup completo de VM do Hyper-V. VM é o acrônimo para **Virtual Machine** <kbd>(Máquina Virtual).</kbd>

Para este artigo foi criada uma VM **(virtual machine)** no Hyper-V do Windows 10 "limpo", ou seja, sem qualquer software adicional instalado,
pois esta máquina virtual será usada somente para testes.

## Criando um backup de uma VM no Hyper-V

Primeiramente, iremos abrir o **“Gerenciador do Hyper-V”**, onde veremos todas as máquinas virtuais existentes.

Clicaremos com o botão direito do mouse em cima da VM que desejamos fazer o backup e, ao abrir o menu suspenso, clicaremos com o botão esquerdo do mouse em **“Exportar”**.

Ou, poderá selecionar a máquina virtual com o botão esquerdo do mouse e clicar em “Exportar” entre as últimas opções de “Ações” na coluna a direita.

Imagem aqui

A seguir, abrirá uma janela onde deveremos clicar em **“Procurar”** para selecionar o local onde desejamos salvar o backup.

Imagem aqui

Neste exemplo, será criada uma pasta de backup dentro do diretório **“Downloads”**.

Para isso, ao abrir a janela **“Selecionar pasta”**, no destino escolhido para salvar, clicaremos em **“Nova pasta”** e colocaremos o nome da pasta para armazenar especificamente o backup da VM.

Se preferirem criar direto no HD Externo ou em um local na rede, também poderá ser feito deste jeito. Apenas levará mais tempo do que armazenar no HD do computador local. Depois só precisa copiar esta pasta para outro local.
