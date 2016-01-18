---
layout: page
title: Tasks
originalLink: https://www.phing.info/docs/stable/hlhtml/index.html#d5e1114
---

Tasks (tarefas) são responsaveis por fazerem o trabalho no Phing. Basicamente,
tasks são ações individuais que o seu script XML pode executar.
Por exemplo, copiar um arquivo, criar um diretório. Mas as tasks podem ser mais
complexas também como **XlstTask** que copia o arquivo e o modifica utilizando
XSLT, **SmartyTask** que faz algo similar utilizando os templates do Smarty, ou
**CreoleTask** que executa SQL no banco de dados.

Tasks suportam os seguintes parâmetros

* Parâmetro simples (strings) passado como atributos de XML ou
* Parâmetros mais complexos que são passados através de tags

Parâmetros simples são basicamente strings. Por exemplo, se você passar um valor