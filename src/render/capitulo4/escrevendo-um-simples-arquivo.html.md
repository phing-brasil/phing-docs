---
layout: page
title: Escrevendo um simples arquivo XML
originalLink: https://www.phing.info/docs/guide/trunk/ch04s02.html
---

```
<project name="FooBar" default="dist">

    <!-- ============================================  -->
    <!-- Target: prepare                               -->
    <!-- ============================================  -->
    <target name="prepare">
        <echo msg="Criando o diretório ./build" />
        <mkdir dir="./build" />
    </target>

    <!-- ============================================  -->
    <!-- Target: build                                 -->
    <!-- ============================================  -->
    <target name="build" depends="prepare">
        <echo msg="Copiando arquivos para o diretório build..." />

        <echo msg="Copiando ./about.php para o diretório ./build..." />
        <copy file="./about.php" tofile="./build/about.php" />

        <echo msg="Copiando ./browsers.php para o diretório ./build..." />
        <copy file="./browsers.php" tofile="./build/browsers.php" />

        <echo msg="Copiando ./contact.php para o diretório ./build..." />
        <copy file="./contact.php" tofile="./build/contact.php" />
    </target>

    <!-- ============================================  -->
    <!-- (DEFAULT)  Target: dist                       -->
    <!-- ============================================  -->
    <target name="dist" depends="build">
        <echo msg="Criando arquivo..." />

        <tar destfile="./build/build.tar.gz" compression="gzip">
            <fileset dir="./build">
                <include name="*" />
            </fileset>
        </tar>

        <echo msg="Arquivos copiados e comprimidos no diretório build!" />
    </target>
</project>
```

O arquivo que utilizamos com o Phing normamente é salvo com o nome build.xml,
que é o nome pdrão pelo qual o Phing irá procurar se nenhum outro nome de arquivo for
especificado.

Para executar o arquivo acima e executar o target padrão (assumindo que o 
arquivo está no diretório atual com o nome padrão build.xml) basta chamar:

```
$ phing
```

O comando a cima irá executar a target especificada no atributo **dist** da tag
**<project>**. Enquanto o arquivo é executado cada tarefa irá exibir alguma 
informação sobre quais ações e quais arquivos foram afetados.

Para executar qualquer target que não seja a padrão informado no atributo **dist**
basta informar o nome do target na linha de comando ao executar o Phing.
Por exemplo para executar o target chamado **build** você deverá executar o
comando 

```
$ phing build
```

## Elemento **project**

O primeiro elemento depois do prólogo do documento é o elemento raiz **< project>**.
Esse elemento é um container para todos os outros elementos e pode/deve ter os 
seguintes atributos:

|Atributo|Descrição|Obrigatório|
|--------|---------|-----------|
|name|O nome do projeto|Não|
|basedir|O diretório base do projeto, use "." para referenciar o diretório atual. Atenção: se nenhum diretório for especificado, o diretório pai do arquivo build.xml é utilizado|Não|
|default|O target padrão a ser executado se nenhum target for especificado|Sim|
|description|A descrição do projeto|Não|

## Elemento **target**

Um target pode depender de outros targets. Você pode ter um target para instalar
os arquivos,por exemplo, e um target para criar um arquivi tar.gz para ser distribuido.
Você só pode criar um arquivo para distribuir após ter instalado os arquivos primeiro,
então o target de distribuição depende do target de instalação. E o Phing resolve
essas dependências.