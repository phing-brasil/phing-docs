---
layout: page
title: Project
originalLink: https://www.phing.info/docs/stable/hlhtml/index.html#d5e1084
---

O objetivo desse capítulo é fazer você se familiarizar com os componentes básicos
de um arquivo XML utilizado pelo Phing. Após ler esse capítulo você poderá entender
a estrutura básica de qualquer arquivo XML que o Phing utiliza até mesmo se você
não souber o que os pedaços individuais fazem.

## Project

Na estrutura do arquivo XML deve exsitir exatamente uma única tag **project**
definida. **```<project>```** é o elemento raiz do arquivo, o que significa que
todos os componentes devem estar dentro da tag **```<project>```**

```
<?xml version="1.0"?>

<project name="teste" description="Arquivo simples de teste" default="main" >
  <!-- Outros elementos -->
<project>
```

O exemplo acima nos mostra a tag project que possui todos atributos utilizados
por essa tag. O atributo ***name*** e ***description*** eles se explicam
apenas pelo nome, o atributo ***default*** especifica o target a ser executado 
se nenhum target for informado.

A partir da versão 2.4.2 é possível utilizar o atributo *phingVersion* na tag
**```<project>```**. Esse atributo permite definir qual a versão mínima do Phing
para executar o arquivo XML para prevenir problemas de compatibilidades.

```
<?xml version="1.0"?>

<project name="teste" phingVersion="2.4.2" >
  <!-- Outros elementos -->
<project>
```

## Componentes do projeto em geral

Todos os elementos dentro da tag project são considerados componenetes de projeto
como por exemplo targets, tasks, types, etc. Os componentes do projeto podem
ter atributos e tags. Atributos possuem apenas valores simples como por exemplo
strings, inteiros etc. Elementos aninhados podem ser tipos complexos do Phing
como **FileSets** ou apenas classes simples para valores com chaves customizadas.