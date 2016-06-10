---
layout: page
title: Escrevendo mappers
originalLink: https://www.phing.info/docs/stable/hlhtml/index.html#d5e2009
---

Escrever sua classe mapper para nomes de arquivos permitirá que você controle como os nomes serão transformados 
nas tasks como CopyTask, MoveTask, XLSTTask, etc. Em alguns casos você pode querer estender mappers existentes 
(por exemplo, criar um GlobMapper que também transforme em maísculas); em outros caos, você pode simplesmente 
querer criar uma transformação de nomes bem simples que não é facilmente realizada por outros mappers como 
GlobMapper ou RegexMapper.

## Criando um mapper

Escrever mappers para nomes de arquivos foi simplificado pelo suporte a interfaces do PHP5. Basicamente, seu mapper 
customizado deve implementar phing.mappers.FileNameMapper. Segue um exemplo de um mapper de nomes de arquivos que 
cria arquivos com nomes no estilo DOS. Para este exemplo, os atributos "to" e "from" não são necessários porque todos 
os arquivos serão transformados. Para ver os atributos "to" e "from" em ação, de uma olhada em phing.mappers.GlobMapper 
ou phing.mappers.RegexpMapper.

```php
require_once "phing/mappers/FileNameMapper.php";

/**
 * Um mapper que cria aqueles horríveis nomes de arquivo padrão DOS
 */
class DOSMapper implements FileNameMapper {

  /**
   * O método main() realiza o mapeamento.
   *
   * Neste caso nós transformamos $sourceFilename em
   * um nome DOS-compatível. Por exemplo:
   * ExtendingPhing.html -> EXTENDI~.DOC
   *
   * @param string $sourceFilename O nome a ser convertido.
   * @return array Os nomes compatíveis.
   */
  public function main($sourceFilename) {

    $info = pathinfo($sourceFilename);
    $ext = $info['extension'];
    // pega o nome do arquivo sem a extensão
    $bname = preg_replace('/\.\w+\$/', '', $info['basename']);

    if (strlen($bname) > 8) {
      $bname = substr($bname,0,7) . '~';
    }

    if (strlen($ext) > 3) {
      $ext = substr($bname,0,3);
    }

    if (!empty($ext)) {
      $res = $bname . '.' . $ext;
    } else {
      $res = $bname;
    }

    return (array) strtoupper($res);
  }

  /**
   * O atributo "from" não é utilizado, mas o método deve existir.
   */
  public function setFrom($from) {}

  /**
   * O atributo "to" não é utilizado, mas o método deve existir.
   */
  public function setTo($to) {}

}
```

## Usando o mapper

Assumindo que este mapper esteja salvo em myapp/mappers/DOSMapper.php (relativo a um path no include_path do PHP ou 
na variável PHP_CLASSPATH do env), então você faria referência a ele dessa maneira no seu arquivo de build: 

```xml
<mapper classname="myapp.mappers.DOSMapper"/>
```
