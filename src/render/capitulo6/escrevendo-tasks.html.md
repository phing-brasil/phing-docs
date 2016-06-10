---
layout: page
title: Escrevendo tasks
originalLink: https://www.phing.info/docs/stable/hlhtml/index.html#d5e1862
---

## Criando uma task

Vamos começar criando uma task simples que não faz nada além de exibir uma mensagem na tela. Veja abaixo o código e 
o XML que serão usados para esta task.

```php
<?php

require_once "phing/Task.php";

class MyEchoTask extends Task {

    /**
     * A mensagem passada no arquivo de build
     */
    private $message = null;

    /**
     * O setter para o atributo "message"
     */
    public function setMessage($str) {
        $this->message = $str;
    }

    /**
     * O método de inicialização: Executa os passos iniciais.
     */
    public function init() {
        // nothing to do here
    }

    /**
     * O método de entrada
     */
    public function main() {
        print($this->message);
    }
}

?>
```

Este código contém uma task simples, porém completa. O arquivo deve ser nomeado MyEchoTask.php. Para este exemplo, 
nós assumimos que este arquivo está localizado na pasta /home/example/classes. Nós iremos explicar o código fonte em 
detalhes, mas primeiro vamos mostrar como devemos registrar a task no Phing para que ela possa ser executada durante 
o processo de build.

## Usando a task

A task mostrada abaixo deve ser carregada e chamada pelo Phing. Além disso ela deve estar disponível para o Phing para 
que o arquivo de build relacione seus elementos e parâmetros XML. Dê uma olhada nesse arquivo de build minimalista que 
faz exatamente isso.

```xml
<?xml version="1.0" ?>

<project name="test" basedir="." default="test.myecho">
    <includepath classpath="/home/example/classes" />
    <taskdef name="myecho" classname="MyEchoTask" />

    <target name="test.myecho">
      <myecho message="Hello World" />
    </target>
</project>
```

Para registrar uma task customizada com o Phing, o elemento taskdef (linha 5) é utilizado. Opcionalmente, antes do 
elemento taskdef, o elemento includepath adiciona um path ao include_path do PHP.

Após registrarmos a task atribuindo um nome e a classe relacionada, ela está pronta para uso dentro do contexto 
<target> (linha 8). Veja que passamos a mensagem que nossa task deve exibir na tela através de um atributo XML 
chamado "message".

## Discutindo o código

Agora que já sabemos como executar uma task em um arquivo de build, é hora de discutir como tudo funciona.

## Estrutura da task

Todos os arquivos que contém a definição de uma task seguem uma estrutura comum:

+ Comandos Include/require para importar todas as classes necessárias

+ Definição e declaração da classe

+ Propriedades da classe

+ Construtor da classe

+ Métodos setters para cada atributo XML

+ O método init()

+ O método main()

+ Métodos private (ou protected) arbitrários

## Includes

Sempre importe todas as classes necessárias para a task. Alé, disso, você sempre deve importar phing/Task.php 
no começo do seu bloco de inclusão. Então inclua todas as outras classes de sistema ou proprietárias.

## Declaração de classes

Na linha 5 do bloco PHP você irá encontrar a declaração da classe. Vai lhe parecer familiar se você tem experiência 
com POO no PHP (vamos assumir que você é). Além disso existem algumas regras que você deve obedecer ao criar classes:

+ O nome da classe deve ser exatamente igual ao da task que você irá implementar mais o sufixo "Task". No nosso 
exemplo o nome da classe é MyEchoTask (construído pelo nome da task "myecho" mais o sufixo "task"). Utilizamos 
maíusculas/minúsculas é para uma melhor leitura, porém encorajamos que você siga este padrão.

+ A classe task que você criar deve estender a classe "Task" para herdar todos os seus métodos específicos.

## Propriedades da classe

Depois da declaração da classe vem as propriedades. A maioria delas é herdada da classe pai Task, então não há 
necessidade de redeclará-las. Você deve declarar as seguintes propriedades:
 
+ Taskname. Sempre especifique a propriedade taskname que é a mesma do elemento XML que sua task utiliza. 
Hoje essa informação não é utilizada - mas será no futuro.

+ Suas propriedades arbitrárias que refletem os atributos/elementos do XML que a sua task aceita.

No exemplo MyEchoTask as propriedades podem ser encontradas entre as linhas 7 e 11. Utilize nomes descritivos 
que deixam claro seu papel no contexto da task. Algumas propriedades herdadas da classe pai não devem ser 
declaradas na parte de propriedades do código.

Uma lista das propriedades herdadas (a maioria delas reservada, então certifique-se de não sobrescrevê-las) pode 
ser encontrada na "API de referência do Phing" no diretório docs/api/.

## O construtor

O próximo bloco é o construtor da classe. Ele deve estar presente e chamar, pelo menos, o construtor da classe pai. 
É claro que você pode adicionar dados de inicialização aqui. É recomendado que você defina aqui o valor das 
propriedades que você declarou.

## Métodos setter

Como você pode ver no XML de definição da nossa task (linha 9), há um atributo definido com a própria task, chamado 
"message" com o texto que a nossa classe deve exibir. A task deve de alguma maneira receber o nome e o valor do 
atributo. Para isso exite o método setter.

Para cada atributo que você quiser importar para o namespace da task você deve definir um método nomeado a partir 
do atributo com o prefixo "set". Este método aceita exatamente um parâmetro que contém o valor do atributo. Assim 
você pode definir a propriedade da classe com o valor que é setado a partir do método setter.

No método seter você deveria também executar qualquer operação de cast ou de verificação se o valor é válido. Se 
for o caso, retorne uma exceção BuildException. Em alguns casos, como quando você tem três atributos e pelo menos 
um deles deve ser setado, você pode checar os valores dos atributos nos métodos init() ou main().

No nosso exemplo o setter é chamado setMessage, porque o atributo que a nossa task recebe é "message". setMessage 
então pega a string "Hello World" provida pelo parser e seta o valor na propriedade $strMessage da classe. O valor 
está agora disponível para uso na nossa task.

## Métodos criadores

São métodos que permitem que você gerencie tags aninhadas do XML na sua nova task do Phing.

## O método init()

O método init deve ser implementado mesmo quando ele não faz nada, como no nosso exemplo. Você pode fazer suas 
inicializações de setup da sua task. Depois de chamar o método init o objeto task continua o mesmo para o parser. 
O método init não deve executar operações relacionadas a ações da task. Um exemplo de utilização do init seria 
tratar o valor da variável $strMessage (por exemplo, trim($strMessage)) ou importar workers adicionais que serão 
necessários para a task.

O método init deve retornar true ou um objeto de erro. Se você não implementar o método init o Phing irá retornar 
um fatal error.

## O método main()

Existe exatamete um único ponto de entrada para executar a task. Ele é chamado após o arquivo buildfile ter sido 
completamente interpretado e todos targets e tasks tenham sido agendados para execução. A partir deste ponto 
começa a implementação das ações da task. No caso do nosso exemplo, uma mensagem (importada pelo método setter) é 
logada na tela através do serviço "Logger" do sistema (é a ação para que a task foi escrita). A chamada do método 
Log() neste caso aceita dois parâmetros: uma constante de evento e a mensagem para o log.

## Métodos arbitrários

Para casos mais ou menos simples (como o nosso exemplo) toda a lógica da task é feita no método main(). Porém, para 
tasks mais complexas o bom senso indica que uma ação deve ser dividida em partes menores de código. O meio mais comum 
de se fazer isso é separar a lógica em métodos privados da classe - e em casos mais complexos, separar em uma 
biblioteca.

```php
private function myPrivateMethod() {
    // definition
}
```