---
layout: page
title: Estrutura do Phing
originalLink: https://www.phing.info/docs/stable/hlhtml/index.html#d5e1706
---

## Arquivos e diretórios

Antes de começar a estender o Phing, vamos dar uma olhada na sua estrutura. Você deve se familiarizar com a organização 
de arquivos na estrutura de diretórios do Phing antes de começar a codificar. Depois de extrair a distribuição inicial ou fazer 
**checkout** do git, você verá a seguinte estrutura de diretórios:

```dir
$PHING_HOME
  |-- bin
  |-- classes
  |    `-- phing
  |         |-- filters
  |         |    `-- util
  |         |-- mappers
  |         |-- parser
  |         |-- tasks
  |         |    |-- ext
  |         |    |-- system
  |         |    |    `-- condition
  |         |    `-- user
  |         `-- types
  |-- docs
  |    `-- phing_guide
  `-- test
       |-- classes
       `-- etc
```

A tabela a seguir descreve resumidamente o conteúdo dos principais diretórios:

Tabela 6.1: Estrutura de diretórios do Phing

|Directory|Contents|
|---------|--------|
|bin|Aplicações básicas (phing, configuração) assim como scripts empacotados para plataformas distintas(atualmente Unix e Windows).|
|classes|Repositório de todas as classes utilizadas pelo Phing. Este é o diretório base que deveria estar no include_path do PHP. Neste diretório você encontrará o subdiretório phing/ com todas as classes relevantes para o Phing.|
|docs|Arquivos de documentação. Livros gerados, manuais online assim como a documentação da API gerada em PHPDoc.|
|test|Um conjunto de casos de teste para diferentes tarefas, mappers e tipos. Se você estiver desenvolvendo no git você deveria adicionar um caso de teste para cada implementação que você fizer o check in.|

Atualmente não há distinção entre os leiautes source e o build do Phing. O leiaute de diretórios mostra a árvore de diretórios 
que contém alguns arquivos adicionais como a página do Phing. Mais tarde pode haver um arquivo de build para criar uma 
estrutura de diretórios vazia do próprio Phing.

## Convenções de nomenclatura

Existem algumas convenções de nomenclatura de arquivos utilizadas pelo Phing. Aqui está um breve resumo das convenções 
básicas:

* Os nomes de arquivo devem ser compostos de exatamente dois elementos: nome e extensão;
* Escolha nomes descritivos porem curtos (devem ter menos de 31 caracteres);
* Os nomes não devem conter pontos.
* Arquivos contendo código PHP devem ter a extensão .php;
* Deve haver pelo menos uma classe por arquivo (métodos procedurais não são aceitos, utilize um arquivo separado para 
eles), com exceção de classes "inner"-type / helpers que podem ser declaradas no mesmo arquivo que as classes 
"outer"-type / principais;
* O arquivo deve ser nomeado exatamente igual ao nome da sua classe.
* Arquivos de build e conjuntos de regras de configuração devem ter a extensão .xml.

## Padrões de código

Nós utilizamos os padrões de código PEAR. Nós usamos uma versão menos restrita destes padrões, mas nós insistimos que 
novas contribuições tenham comentários **PHPDoc** e que façam declarações explícitas de variáveis e métodos 
**public/protected/private**.  Se você tiver sugestões sobre melhorias para o código base do Phing, não hesite em nos contatar.