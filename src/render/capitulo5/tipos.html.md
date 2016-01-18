---
layout: page
title: Tipos
originalLink: https://www.phing.info/docs/stable/hlhtml/index.html#sec.types
---

Existem os tipos básicos (strings, inteiros, boolean) que você pode utilizar como parâmetro em uma task. Mas também 
existem tipos complexos que são utilizados através de tags aninhados (tags dentro de outra tag)

``` xml
<task>
  <tipo />
</task>

<!-- ou: -->

<task>
  <tipo>
    <subtipo>
      <!-- etc. -->
    </subtipo>
  </tipo>
</task>
```

Note que os tipos podem obter mais de um nível de aninhamento, como mostra o segundo exemplo acima

## Referenciando tipos

