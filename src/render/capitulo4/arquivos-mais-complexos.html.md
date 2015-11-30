---
layout: page
title: Arquivos mais complexos
originalLink: https://www.phing.info/docs/stable/hlhtml/index.html#d5e985
---

```
<?xml version="1.0"  encoding="UTF-8" ?>

<project name="testsite" basedir="." default="main">
    <property file="./build.properties" />

    <property name="package"  value="${phing.project.name}" override="true" />
    <property name="builddir" value="./build/testsite" override="true" />
    <property name="srcdir"   value="${project.basedir}" override="true" />

    <!-- Fileset for all files -->
    <fileset dir="." id="allfiles">
        <include name="**" />
    </fileset>

    <!-- ============================================  -->
    <!-- (DEFAULT) Target: main                        -->
    <!-- ============================================  -->
    <target name="main" description="main target">
        <copy todir="${builddir}">
            <fileset refid="allfiles" />
        </copy>
    </target>

    <!-- ============================================  -->
    <!-- Target: Rebuild                               -->
    <!-- ============================================  -->
    <target name="rebuild" description="rebuilds this package">
        <delete dir="${builddir}" />
        <phingcall target="main" />
    </target>
</project>
```
Esse arquivo de configuração primeiramente define algumas propriedades com a tag **```<property>```**.
E então é definido um **fileset** e dois targets. Vamos entrar em detalhes sobre cada item no 
arquivo XML.

As primeiras 5 tags definem tags de propriedades. Elas aparecem em duas maneiras:


* A segunda propriedade possui apenas um atributo file. O caminho do arquvo 
informado tem que ser relativo ou absoluto

* E a outra maneira, a tag possui um atributo name e um atributo value. Após a chamada
os valores definidos podem ser acesso através da sintaxe ```"${" and "}"```.

A próxima coisa a se notar no arquivo é a tag **```<fileset>```**. Essa tag define um 
conjuntos de arquivos. Você pode incluir ou exclur arquivos com as tags **include** 
e **exclude**. Para maiores informações sobre fileset veja o apêndice D. O fileset
utilizado no exemplo possui o atributo id para que seja possível referenciar
esse fileset posteriormente.

Uma coisa que devemos nos atentar é ao uso das estrelas duplas ```"**"```. Essa regexp
especial refere-se a todos os arquivos em todos os subdiretórios. Compare isso com
apenas uma estrela ```"*"``` que refere-se a todos os arquivos no subdiretório atual.
Por exemplo a expressão ```"**/*.phps"``` refere-se a todos os arquivos com o sufixo
**```".phps"```** em todos os subdiretórios corrente.

A primeira tarefa possue apenas uma chamada á tarefa **CopyTask** através da tag 
**```<copy>```**. A parte interessante aqui é o que tag copy possui dentro dela. Aqui, 
a tarefa fileset não é escrita com elementos include ou exclude mas sim o o atributo
refid, o fileset criado anteriormente é referenciado. Dessa forma você pode utilizar
varias vezes um fileset que foi definido apenas uma vez.

A unica coisa notável no segundo target é a chamada da tarefa **PhingTask** com a tag
**```<phingcall>```** (Veja o apêndice B para maiores informações). A tarefa executa um 
target específico no mesmo arquivo XML. O que irá acontecer é que o segundo target
irá remover o diretório build e chamar o target main de novo.

Podemos sobrescrever propriedades definidas no arquivo XML utilizando o argumento -D.
Por exemplo para sobrescrever o builddir no arquivo XML acima poderiamos chamar o 
Phing da seguinte forma

```
$ phing -Dbuilddir=/tmp/system-test  
```