---
title: A história: Phing e Binarycloud
layout: page
originalLink: https://www.phing.info/docs/stable/hlhtml/index.html#d5e511
---

O Phing era originalmente um subprojeto da Binarycloud. Binarycloud é um framework de aplicação altamente projetado, 
que foi pensado para o uso em ambiente corporativo. Binarycloud usa bastante o XML para armazenar metadados sobre o projeto 
(configurações, nodes, widgets, estrutura do site, etc.). Porque o Binarycloud foi feito para o PHP, executar diversos 
processamentos e transformações XML em cada requisição de página é uma proposta irrealista. O Phing é utilizado para "compilar" 
os metadados XML em arrays PHP que podem ser processados sem sobrecarregar os scripts PHP.

É claro que a compilação do XML é apenas uma das várias maneiras em que o Binarycloud utiliza o sistema de build do Phing. 
O sistema de build do Phing torna possível para você: 

* Construir páginas multi-idioma a partir de uma estrutura;
* Centralizar metadados (por exemplo, seu modelo de dados) em um arquivo XML e gerar diversos arquivos a partir deste XML, 
com diferentes XSLT.

No começo, o Binarycloud utilizava o sistema de make do GNY; porém, esta abordagem tinha alguns inconvenientes: 
O problema do espaço-antes-do-tab nos arquivos make, o fato de que ele só era nativo em plataformas Unix, etc. Então surgiu 
a necessidade de um sistema melhor de build. Por causa de seus arquivos de build em XML e de seu design modular, o Apache Ant 
foi a escolha lógica. O problema é que o Ant é escrito em Java, então você precisa instalar uma JVM no seu computador para 
usá-lo. Além da necessidade se usar um outro interpretador (além do PHP), havia também um conflito legal/ideológico em 
obrigar o uso de uma JVM comercial (havia problemas com a utilização do Ant em outras JVMs além da da Sun) para 
um Binarycloud LGPL.

Então o desenvolvimento do Phing começou. O Phing é um sistema de build escrito em PHP e que utiliza ideias do Ant. 
A primeira versão foi projetada e desenvolvida simultaneamente, e portanto não era tão sofisticada. Este sistema original 
foi rapidamente levado ao seu limite e a necessidade de um Phing melhor se tornou uma prioridade. Andreas Aderhold, que 
foi o responsável pelo Phing/r1, desenvolveu e escreveu muito do Phing/r2 que veio a seguir. Phing/r2 se tornou o 
Phing-1.0 que ainda existe hoje para PHP4.

A versão de desenvolvimento atual do Phing, 2.x, requer PHP5 (pelo menos a versão 5.2.x) e utiliza muitos dos recursos 
disponíveis no PHP 5.2 para atingir um alto nível de modularização, eficiência de c´digo assim como estabilidade e 
testabilidade.