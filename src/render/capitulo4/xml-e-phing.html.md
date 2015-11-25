---
layout: page
title: XML e Phing
originalLink: https://www.phing.info/docs/stable/hlhtml/index.html#ch.gettingstarted
---

Os arquivos utilizados pelo Phing são escritos em XML, e você irá precisar saber
pelo menos algumas coisas básicas sobre XML para entender esse capítulo. Existe
muita informação na web:

* A recomendação oficial do XML pelo W3C :http://www.w3.org/TR/2000/REC-xml: Muito técnico

* 10 pontos do XML http://www.w3.org/XML/1999/XML-in-10-points : Rápida introdução

* Introdução técnica ao XML http://www.xml.com/pub/a/98/10/guide0.html : Artigo interessante
escrito pelo criador do DokBook

## XML e Phing

Um arquivo válido para usar com o Phing possui a seguinte estrutura :

* O prólogo do documento

* Exatamente um elemento chamado **<project>**

* Muitos elementos do Phing (**< property\>**, **< fileset\>**, **<patternset\>** etc)

* Um ou mais elementos **<target>** possuindo tarefas do Phing ou tarefas definidas
pelo usuário (**< install\>**, **< bcc\>** etc)