---
layout: page
title: Usando o Phing
originalLink: https://www.phing.info/docs/guide/trunk/ch03s05.html
---

Agora você está preparado para executar o Phing na linha de comando ou através 
de scripts. O conteúdo a seguir descreve brevemente como executar o Phing
corretamente

## Linha de comando

Phing execution on the command line is simple. Just change to the directory where 
your buildfile resides and type

A execução através da linha de comando é fácil. Apenas vá aonde o seu arquivo XML
está localizado e execute

```
$ phing [target [target2 [target3] ...]]
```

[target ...] são os alvos que você deseja executar. Se nenhum target for especificado
Phing irá tentar executar o target padrão especificado na tag **project**.

```
<project default="targetPadrao">
    <target name="targetPadrao">
    </target>
</project>
```

Quando Phing é executado para executar multiplos targets, Phing invocará cada
target independentemente de outros targets.

```
$ phing meuTarget1 meuTarget2
```
