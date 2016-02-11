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

Uma caracteristica se notar é a possibilidade de referênciar tipos através dos ids, após a definição do mesmo em algum lugar do seu arquivo XML. Veja o nosso exemplo :

```
<project>

  <!-- Define o tipo foo para ser usado posteriormente -->
  <fileset id="foo">
    <include name="*.php" />
  </fileset>

  <!-- Target que utiliza o tipo foo -->
  <target name="copy" >
    <copy todir="/tmp">
      <fileset refid="foo" />
    </copy>
  </target>
</project>
```

Como você pode ver, o tipo contém um atributo id e é esse atributo que é utilizado para se referenciar ao tipo criado. No exemplo acima temos a uma tarefa que realiza a cópia de todos os arquivos com a extensão **.php** para o diretório **/tmp**. Veja que o tipo é criado através da tag `<fileset>` e é dado o atributo id o valor **foo**.

Logo em seguida no target **copy** temos a tarefa **copy** que efetivamente irá copiar os arquivos, repare que dessa vez apenas referenciamos os arquivos criados anteriormente através do atributo **refid**.