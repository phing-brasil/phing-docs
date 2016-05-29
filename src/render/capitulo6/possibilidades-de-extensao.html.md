---
layout: page
title: Possibilidades de extensão
originalLink: https://www.phing.info/docs/stable/hlhtml/index.html#d5e1687
---

Existem 3 áreas principais em que o Phing pode ser estendido: **Tasks**, tipos e **mappers**. As sessões a seguir discutem estas 
opções.

## **Tasks**

Tasks são trechos de código que executam uma ação únicam, como instalar um arquivo. Portanto, uma classe **worker** a ser 
criada e armazenada em um local específico, que implementa o trabalho. A classe **worker** é somente uma interface para o 
Phing que deve cumprir alguns requisitos, que serão discutidos adiante nesse capítulo, contudo ela pode - mas não 
necessariamente deve - usar outras classes, **workers** e bibliotecas que ajudam a executar as operações necessárias.

## Tipos

Extender tipos é uma necessidade rara; Ainda assim, você pode fazê-lo. Um possível tipo que você pode implementar é uma 
**urlset**, por exemplo.

Você pode acaber precisando de um novo tipo para uma **task** que você escreveu; por exemplo, se você estiver escrevendo o 
**XSLTTask** você pode descobrir que você vai precisar de um tipo especial para o **XSLTParams** (mesmo que nesse caso você 
poderia usar o tipo de parâmetro genérico nome/valor). Em casos onde o tipo é realmente exclusivo de uma única tarefa, você 
pode querer apenas definir a classe de tipo no mesmo arquivo que a classe **Tasks**, ao invés de criar um tipo oficial 
**stand-alone**.

## **Mappers**

Criar novos **mappers** é também uma necessidade rara, uma vez que quase tudo pode ser manipulado pelos **Core mappers**. O 
**framework** **mapper** provê um jeito simples para definir seus próprios **mappers** e eles implementam uma interface muito 
simples.