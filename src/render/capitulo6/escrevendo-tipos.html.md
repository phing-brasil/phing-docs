---
layout: page
title: Escrevendo tipos
originalLink: https://www.phing.info/docs/stable/hlhtml/index.html#d5e1976
---

Você só deve criar novos tipos se o tipo precisa ser compartilhado com mais de uma task. Se o tipo só é utilizado 
em uma task específica - por exemplo, manipular um parâmetro ou outra tag necessária para a task - então a classe 
Type deve ser definida no mesmo arquivo que a task (Por exemplo, phing/filters/XSLTFilter.php também inclui uma 
classe XSLTParam que não é usada em nenhum outro lugar).
 
Para casos onde você precisa de tipos mais genéricos, você pode criar sua própria classe Type - similar ao jeito 
de ser criar uma task.

## Criando um Tipo de dados


Classes Type precisam estender a classe abstrata DataType. Além de prover meios de categorizar tipos, a classe 
DataType provê os métodos necessários para suportar o atributo "refid" (Todos os tipos podem ter um id, e eles 
podem ser referenciados mais tarde utilizando esse id).

Nesse exemplo nós criamos um tipo DSN porque nós escrevemos várias tasks relacionadas a banco, e cada uma delas 
precisa saber como se conectar ao banco; ao invés de ter parâmetros de banco de dados para cada task, vamos criar 
um tipo DSN para podermos identificar os parâmetros da conexão apenas uma vez e então usá-los em todas as nossas 
tasks.

```php
require_once "phing/types/DataType.php";

/**
 * Este tipo representa uma conexão de banco de dados
 */
class DSN extends DataType {

  private $url;
  private $username;
  private $password;
  private $persistent = false;

  /**
   * Define a parte da URL: mysql://localhost/mydatabase
   */
  public function setUrl($url) {
    $this->url = $url;
  }

  /**
   * Define o usuário a ser utilizado na conexão
   */
  public function setUsername($username) {
    $this->username = $username;
  }

  /**
   * Define a senha a ser utilizada na conexão
   */
  public function setPassword($password) {
    $this->password = $password;
  }

  /**
   * Define se será utilizada uma conexão persistente
   * @param boolean $persist
   */
  public function setPersistent($persist) {
    $this->persistent = (boolean) $persist;
  }

  public function getUrl(Project $p) {
    if ($this->isReference()) {
      return $this->getRef($p)->getUrl($p);
    }
    return $this->url;
  }

  public function getUsername(Project $p) {
    if ($this->isReference()) {
      return $this->getRef($p)->getUsername($p);
    }
    return $this->username;
  }

  public function getPassword(Project $p) {
    if ($this->isReference()) {
      return $this->getRef($p)->getPassword($p);
    }
    return $this->password;
  }

  public function getPersistent(Project $p) {
    if ($this->isReference()) {
      return $this->getRef($p)->getPersistent($p);
    }
    return $this->persistent;
  }

  /**
   * Retorna um hash/array combinado para o DSN como o utilizado pelo PEAR.
   * @return array
   */
  public function getPEARDSN(Project $p) {
    if ($this->isReference()) {
      return $this->getRef($p)->getPEARDSN($p);
    }

    include_once 'DB.php';
    $dsninfo = DB::parseDSN($this->url);
    $dsninfo['username'] = $this->username;
    $dsninfo['password'] = $this->password;
    $dsninfo['persistent'] = $this->persistent;

    return $dsninfo;
  }

  /**
   * Seu tipo deve implementar esta função, que certifica que
   * não há referências circulares e que as referências são do 
   * tipo correto(DSN neste exemplo).
   *
   * @return DSN
   */
  public function getRef(Project $p) {
    if ( !$this->checked ) {
      $stk = array();
      array_push($stk, $this);
      $this->dieOnCircularReference($stk, $p);
    }
    $o = $this->ref->getReferencedObject($p);
    if ( !($o instanceof DSN) ) {
      throw new BuildException($this->ref->getRefId()." doesn't denote a DSN");
    } else {
      return $o;
    }
  }

}
```

## Usando o tipo

O TypedefTask provê uma maneira de declarar seu tipo para que você possa usá-lo no seu arquivo de build. Neste exemplo 
está mostrado como você usaria esse tipo para definir um único DSN e usá-lo em múltiplas tasks (É claro que você poderia 
especificar os parâmetros da conexão DSN todas as vezes, mas as premissas por trás de se ter um tipo é evitar especificar 
a os parâmetros da conexão para cada task).

```xml
<?xml version="1.0" ?>

<project name="test" basedir=".">

  <typedef name="dsn" classname="myapp.types.DSN" />

  <dsn
      id="maindsn"
      url="mysql://localhost/mydatabase"
      username="root"
      password=""
      persistent="false" />

  <target name="main">

    <my-special-db-task>
         <dsn refid="maindsn"/>
    </my-special-db-task>

    <my-other-db-task>
      <dsn refid="maindsn"/>
    </my-other-db-task>

  </target>

</project>
```

## Discussão do Código
 
### Getters e Setters

Você deve prover um método seter para cada atributo que você quiser passar no seu arquivo de build. É uma 
boa prática também definir um método getter, mas na prática você decide como suas tasks irão usar seu tipo.
No exemplo acima nós definimos um método getter para cada atributo e também definimos um método getter adicional: 
DSN::getPEARDSN() que retorna o hash DSN para ser utilizado pelo PEAR::DB, PEAR::MDB e Creole. Dependendo da 
necessidade da task, nós podemos definir apenas o método getPEARDSN() ao invés de definir um método para cada 
atributo.

Também é importante notar que o método getter deve checar se o tipo atual é uma referência a um tipo previamente 
definido - o método DataType::isReference() serve para este propósito. Por isso, o método getter deve ser chamado 
no projeto atual, porque as referências são relativas a um projeto.

### O método getRef()

O método getRef() precisa ser implementado no seu tipo. Este método é responsável por retornar um objeto referenciado; 
ele precisa se certificar que o objeto referenciado é do tipo correto (por exemplo, você pode tentar referenciar uma 
RegularExpression a partir de um tipo DSN) e que a referência não é circular.

Você pode provavelmente copiar este método a partir de um tipo existente e fazer algumas modificações para 
customizá-lo para o seu tipo.