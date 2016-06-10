---
layout: page
title: Ciclo de vida do build
originalLink: https://www.phing.info/docs/stable/hlhtml/index.html#d5e1824
---

Esta seção existe para explicar - ou tentar - como o Phing "funciona". Particularmente,como o Phing processa o arquivo build e 
invoca tasks e tipos baseado nas tags que ele encontra.

## Como o Phing interpreta os arquivos de build

O Phing usa uma classe ExpatParser e funções nativas do PHP para interpretação de XML para manipular os arquivos de 
build. Todas as classes de manipulação estendem a classe phing.parser.AbstractHandler. Estas classes manipulam as tags 
que são encontradas no arquivo de build.

Tasks principais e tipos são mapeados para tags XML nos arquivos defaults.properties - especificamente 
phing/tasks/defaults.properties e phing/types/defaults.properties.

Basicamente, funciona assim:

1. phing.parser.RootHandler é registrado para manipular o documento XML de build.

2. RootHanlder espera encontrar exatamente um elemento: <project>. Ele invoca o ProjectHandler com os atributos da 
tag <project> ou retorna uma exceção se <project> não for encontrado ou se alguma coisa for encontrada em seu lugar.

3. ProjectHandler espera encontrar tags <target>; para elas ele invoca o TargetHandler. Ela também tem exceções para 
manipular certas tarefas que podem ser executadas com prioridade: <resolve>, <taskdef>, <typedef>, e <property>; 
para estas ele invoca a classe TaskHandler. Se uma tag apresentada não corresponde às tags esperadas, ele assume 
que ela é um tipo e invoca a classeDataTypeHandler.

4. TargetHandler espera que todas as tags sejam tarefas ou tipos e invoca o manipulador apropriado (baseado no 
mapeamento especificado nos arquivos defaults.properties).

5. Tasks and tipos podem ter elementos aninhados, mas somente se eles corresponderem a um método create*() na classe 
de tarefa ou tipo. Por exemplo, uma tag aninhada <param> pode corresponder a um método createParam() da task ou do tipo.