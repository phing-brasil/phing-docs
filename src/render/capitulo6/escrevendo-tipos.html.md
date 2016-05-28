---
layout: page
title: Escrevendo tipos
originalLink: https://www.phing.info/docs/stable/hlhtml/index.html#d5e1976
---

You should only create a standalone Type if the Type needs to be shared by more than one Task. If the Type 
is only needed for a specific Task -- for example to handle a special parameter or other tag needed for 
that Task -- then the Type class should just be defined within the same file as the Task. (For example, 
phing/filters/XSLTFilter.php also includes an XSLTParam class that is not used anywhere else.)

For cases where you do need a more generic Type defined, you can create your own Type class -- similar 
to the way a Task is created.

6.7.1 Creating a DataType

Type classes need to extend the abstract DataType class. Besides providing a means of categorizing types,
the DataType class provides the methods necessary to support the "refid" attribute. (All types can be given
 an id, and can be referred to later using that id.)

In this example we are creating a DSN type because we have written a number of DB-related Tasks, each of 
which need to know how to connect to the database; instead of having database parameters for each task, 
we've created a DSN type so that we can identify the connection parameters once and then use it in all our
 db Tasks.

require_once "phing/types/DataType.php";

/**
 * This Type represents a DB Connection.
 */
class DSN extends DataType {

  private $url;
  private $username;
  private $password;
  private $persistent = false;

  /**
   * Sets the URL part: mysql://localhost/mydatabase
   */
  public function setUrl($url) {
    $this->url = $url;
  }

  /**
   * Sets username to use in connection.
   */
  public function setUsername($username) {
    $this->username = $username;
  }

  /**
   * Sets password to use in connection.
   */
  public function setPassword($password) {
    $this->password = $password;
  }

  /**
   * Set whether to use persistent connection.
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
   * Gets a combined hash/array for DSN as used by PEAR.
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
   * Your datatype must implement this function, which ensures that there
   * are no circular references and that the reference is of the correct
   * type (DSN in this example).
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
6.7.2 Using the DataType

The TypedefTask provides a way to "declare" your type so that you can use it in your build file. Here is how
 you would use this type in order to define a single DSN and use it for multiple tasks. (Of course you could
  specify the DSN connection parameters each time, but the premise behind needing a DSN datatype was to avoid 
  specifying the connection parameters for each task.)


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
6.7.3 Source Discussion

Getters & Setters

You must provide a setter method for every attribute you want to set from the XML build file. It is good 
practice to also provide a getter method, but in practice you can decide how your tasks will use your task. 
In the example above, we've provided a getter method for each attribute and we've also provided an additional
 method:DSN::getPEARDSN() which returns the DSN hash array used by PEAR::DB, PEAR::MDB, and Creole. Depending
  on the needs of the Tasks using this DataType, we may only wish to provide the getPEARDSN() method rather 
  than a getter for each attribute.

Also important to note is that the getter method needs to check to see whether the current DataType is a 
reference to a previously defined DataType -- the DataType::isReference() exists for this purpose. For this
reason, the getter methods need to be called with the current project, because References are stored 
relative to a project.

The getRef() Method

The getRef() task needs to be implemented in your Type. This method is responsible for returning a referenced 
object; it needs to check to make sure the referenced object is of the correct type (i.e. you can't try to 
refer to a RegularExpresson from a DSN DataType) and that the reference is not circular.

You can probably just copy this method from an existing Type and make the few changes that customize it to 
your Type.