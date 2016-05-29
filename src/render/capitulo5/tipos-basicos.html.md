---
layout: page
title: Tipos básicos
originalLink: https://www.phing.info/docs/stable/hlhtml/index.html#d5e1175
---

Essa seção introduz os tipos básicos utilizados no Phing

## Fileset

Fileset são grupos de arquivos. Onde você pode incluir ou excluir arquivos específicos baseado em padrões. A seguir iremos explicar mais sobre padrões, mas por agora se atente ao código baixo:

``` xml
<fileset dir="/tmp" id="fileset1">
  <include name="sometemp/file.txt" />
  <include name="othertemp/**" />
  <exclude name="othertemp/file.txt" />
</fileset>

<fileset dir="/home" id="fileset2">
  <include name="foo/**" />
  <include name="bar/**/*.php" />
  <exclude name="foo/tmp/**" />
</fileset>
```

A uilização de padrões é bem simples: Se você quer incluir uma parte do nome um arquivo ou o nome de um diretório use `*.`, como em nosso próximo exemplo incluímos todos os arquivos com a extensão **.php**

``` xml
<fileset dir="/tmp" id="fileset1">
  <include name="bar/**/*.php" />
</fileset>
```

Se você quer incluir múltiplos diretórios e/ou arquivos use `**`. Dessa forma Filesets fornece uma maneira poderosa de manipular arquivos com Phing

## Filelist

Como Fileset, Filelist são coleções de arquivos, entretanto, Filelist é uma lista definida de arquivos (e os arquivos não precisam efetivamente existir no sistema operacional).

Além de permitir referenciar arquivos não existentes, Filelist nos permite especificar os arquivos em uma determinada ordem. Arquivos em Filset são ordenados pelo sistema operacional, e em alguns casos você vai querer especificar uma lista de arquivos para serem processados em uma determinada ordem

``` xml
<filelist dir="base/" files="file1.txt,file2.txt,file3.txt"/>

<!-- OR: -->
<filelist dir="basedir/" listfile="files_to_process.txt"/>
```