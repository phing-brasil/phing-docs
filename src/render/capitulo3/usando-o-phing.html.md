---
layout: page
title: Usando o Phing
originalLink: https://www.phing.info/docs/guide/trunk/ch03s05.html
---

Agora você está preparado para executar o Phing na linha de comando ou através 
de scripts. O conteúdo a seguir descreve brevemente como executar o Phing
corretamente

## Linha de comando

A execução através da linha de comando é fácil. Apenas vá aonde o seu arquivo XML
está localizado e execute

```
$ phing [target [target2 [target3] ...]]
```

[target ...] são os alvos que você deseja executar. Se nenhum target for especificado
Phing irá tentar executar o target padrão especificado na tag **project**.

```
<!-- Arquivo targets.xml -->
<project default="targetPadrao">
    <target name="segundaTarget">
        <echo msg="Segunda target"/>
    </target>
    
    <target name="targetPadrao">
        <echo msg="Target padrão"/>
    </target>
</project>
```

Dado o arquivo XML acima poderiamos executar a target `targetPadrao` simplesmente executando o Phing

```
phing -f targets.xml
```

Obteriamos o seguinte resultado :

```
targetPadrao.xml > targetPadrao:

     [echo] Target padrão

BUILD FINISHED

Total time: 0.1176 seconds
```

Quando Phing é executado para executar multiplos targets, Phing invocará cada
target independentemente de outros targets, veja que para fazer isso basta informar o nome dos targets separados por espaço.

```
$ phing meuTarget1 meuTarget2
```

Fazendo um comparativo com o mesmo arquivo XML utilizado anteriormente, podemos executar dois targets da seguinte maneira

```
phing -f targetPadrao.xml targetPadrao segundaTarget
```

E obtemos o seguinte resutado

```
targetPadrao.xml > targetPadrao:

     [echo] Target padrão

targetPadrao.xml > segundaTarget:

     [echo] Segunda target

BUILD FINISHED

Total time: 0.0854 seconds
```