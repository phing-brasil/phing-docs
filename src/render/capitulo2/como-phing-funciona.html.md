---
title: Como Phing funciona?
layout: page
link: https://www.phing.info/docs/stable/hlhtml/index.html#d5e531
---

Phing usa arquivos XML que contêm uma descrição das tarefas a serem executadas. 
O arquivo está estruturado em targets (alvos) que contêm os comandos reais para 
serem executados (por exemplo, o comando para copiar um arquivo, deletar um diretório, 
executar uma consulta ao DB, etc.). 

Assim, para usar Phing, você deve escrever seu primeiro arquivo e, 
em seguida, executar o Phing, especificando o alvo em seu arquivo que você 
deseja executar.

``` phing -f meu_arquivo.xml meu_target ```

Por padrão Phing irá procurar um arquivo chamado **build.xml** (assim você não precisa
especificar o nome do arquivo a não ser que não seja build.xml) e se nenhum target
é especificado Phing irá tentar executar o target padrão, especificado na tag <project>.

Targets podem possuir dependências. Elas podem depender de outros targets ou de 
outros arquivo.