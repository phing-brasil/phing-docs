---
layout: page
title: Targets
originalLink: https://www.phing.info/docs/stable/hlhtml/index.html#d5e1114
---

Targets são coleções de componenetes do projeto (mas não outros targets) que são
utilizados com um nome único no projeto. Um target geralmente realiza uma tarefa
específica -- ou invoca outros targets que invocam tarefas especificas -- entretanto
um target é como se parece com uma função (porém um target não possue um valor
de retorno).

Targets provavelmente dependeram de outros targets. Por exemplo, se um target A
depende de um target B, então target A é invocado, mas o target B irá ser executado
primeiro. Phing automaticamente resolve essas dependências. Você não pode
ter referências circulares como por exemplo : target A depende do target B e
o target B depende do target A.

O código abaixo mostra a utilização de targets

```
<target name="outraTarefa" depends="construirPagina" description="Qualquer coisa">
  <!-- Tarefas são invocadas aqui -->
<target>

<target name="construirPagina" description="Alguma descrição">
  <!-- Tarefas são invocadas aqui -->
<target>
```