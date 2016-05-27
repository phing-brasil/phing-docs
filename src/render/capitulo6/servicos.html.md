---
layout: page
title: Tipos básicos
originalLink: https://www.phing.info/docs/stable/hlhtml/index.html#d5e1818
---

6.4.1 The Exception system

Phing uses the PHP5 try/catch/throw Exception system. Phing defines a number of Exception subclasses for more fine-grained handling of Exceptions. Low level Exceptions that cannot be handled will be wrapped in a BuildException and caught by the outer-most catch() {} block.