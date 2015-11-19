---
layout: page
title: Alternativa de instalação
originalLink: https://www.phing.info/docs/stable/hlhtml/index.html#d5e719
---

Se você não está usando PEAR, você vai precisar configurar seu ambiente para utilizar o Phing. A distribuição do Phing consiste de três diretórios: **bin**, **docs** e **classes**. Para instalar o Phing, escolha um diretório e descomprima o arquivo de destribuição nesse diretório. Esse diretório será conhecido como PHING_HOME.

> **Atenção**:
> Em versões do Windows o script usado para abrir o Phing terá problemas o caminho definido para PHING_HOME for longo.
> Isso ocorre graças a uma limitação do sistema operacional em manipular arquivos batch. É recomendado no entanto que
> Phing seja instalado em um caminho curto, como C:\opt\phing.

Antes de utilizar o Phing aqui estão alguns passos necessários que você precisará realizar

* Adicione o caminho completo do Phing ao PATH

* Defina a variável de ambiente PHING_HOME para o diretório onde você instalou o Phing. Em alguns sistemas operacionais o conteiner do Phing pode adivinhar onde o Phing está instalado. Porém é melhor não utilizar esse tipo de alternativa.

* Defina a variável de ambiente PHP_COMMAND apontando para o seu executável do PHP como por exemplo (PHP_COMMAND=/usr/bin/php)

* Defina a variável de ambiente PHP_CLASSPATH. Essa variável deve ser definida apontando para o diretório PHING_HOME/classes. Como alternativa você pode apenas adicionar o diretório phing/classes á configuração do PHP na opção include_path dentro do php.ini

* Verifique o seu php.ini para ter certeza que você possui as seguintes configurações

```
max_execution_time = 0

memory_limit = 32M
```