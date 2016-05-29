---
layout: page
title: Inicialização do Sistema
originalLink: https://www.phing.info/docs/stable/hlhtml/index.html#d5e1778
---

Instalações PHP são geralmente customizadas - por exemplo, diferentes limites de memória, valores de timeout, etc. A 
primeira coisa que o Phing faz é modificar os valores do **PHP INI** para criar um ambiente PHP padrão. Isto é executado pela 
camada de inicialização do Phing, que usa um procedimento de inicialização de três níveis. Basicamente, executando três 
arquivos:

* Script específico para a plataforma na pasta bin/
* Aplicação principal na pasta bin/
* Classe Phing na pasta classes/phing/

Em uma primeira análise pode parecer desnecessário. Por que três níveis de inicialização? A razão principal por essa divisão é 
porque o Phing foi construído para que outros frontends (PHP-GTK, por exemplo) pudessem ser usados no lugar da linha de 
comando.

## Scripts empacotados

Estes scrips tecnicamente não são obrigatórios, mas são disponibilizados para facilitar o uso. Imagine se você tivesse que 
digitar isso toda vez que você quisesse fazer o build do projeto: 

``` php -qC /path/to/phing/bin/phing.php -verbose all distro snapshot ```

De fato não parece muito elegante. Além disso, se você não definiu todas as suas variáveis de ambiente, estes scripts podem 
adivinhar os valores corretos para você. Ainda assim, você deveria definí-las sempre.

Os scripts dependem da plataforma, então você encontrará **scripts shell** para plataformas Unix (sh) assim como os scripts 
batch para a plataforma Windows. Se você definir seu path corretamente você poderá chamar o Phing de qualquer lugar do 
seu sistema com o seguinte comando (referindo ao exemplo acima):

``` phing -v2 all distro ```

## A aplicação principal (phing.php)

Basicamente chama classe Phing que executa toda a lógica para voce. Se você olhar o código fonte do arquivo phing.php, 
você verá que toda a inicialização é feita na classe Phing. O phing.php é simplesmente um arquivo para permitir a chamada do 
Phing na linha de comando.

## A classe Phing

Uma vez que todos os passos anteriores da inicialização foram executados com sucesso, a classe Phing é incluída e a 
inicialização, **Phing::startup()**, é chamada pelo script principal da aplicação. Ela define os componentes do sistema, 
configurações INI, PEAR e algumas outras coisas. O processo de inicialização detalhado é o seguinte: 

* Inicialização do temporizador
* Definição das constantes do sistema
* Definição das configurações INI
* Definição dos caminhos de inclusão

Depois que a aplicação principal completa todas as operações (com sucesso ou não) ela chama 
**Phing::shutdown**(CODIGO_DE_SAIDA), que cuida da destruição correta de todos os objetos e do encerramento correto do 
sistema, retornando um código de saída para utilização do shell.