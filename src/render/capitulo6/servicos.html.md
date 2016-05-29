---
layout: page
title: Serviços
originalLink: https://www.phing.info/docs/stable/hlhtml/index.html#d5e1818
---

## O sistema de exceção

O Phing usa o sistema de exceção **try/catch/throw** do PHP5. Ele define algumas subclasses para maior refinamento ao tratar 
as Exceções. Exceções de baixo nível que não puderem ser tratadas entrarão em uma exceção **BuildException** e serão 
tratadas por um bloco catch externo.