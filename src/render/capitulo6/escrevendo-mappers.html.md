---
layout: page
title: Escrevendo mappers
originalLink: https://www.phing.info/docs/stable/hlhtml/index.html#d5e2009
---

Writing your own filename mapper classes will allow you to control how names are transformed in tasks 
like CopyTask, MoveTask, XSLTTask, etc. In some cases you may want to extend existing mappers (e.g. creating 
a GlobMapper that also transforms to uppercase); in other cases, you may simply want to create a very specific 
name transformation that isn't easily accomplished with other mappers like GlobMapper or RegexpMapper.

6.8.1 Creating a Mapper

Writing filename mappers is simplified by interface support in PHP5. Essentially, your custom filename mapper 
must implement phing.mappers.FileNameMapper. Here's an example of a filename mapper that creates DOS-style 
file names. For this example, the "to" and "from" attributes are not needed because all files will be transformed. 
To see the "to" and "from" attributes in action, look at phing.mappers.GlobMapper or phing.mappers.RegexpMapper.

require_once "phing/mappers/FileNameMapper.php";

/**
 * A mapper that makes those ugly DOS filenames.
 */
class DOSMapper implements FileNameMapper {

  /**
   * The main() method actually performs the mapping.
   *
   * In this case we transform the $sourceFilename into
   * a DOS-compatible name.  E.g.
   * ExtendingPhing.html -> EXTENDI~.DOC
   *
   * @param string $sourceFilename The name to be coverted.
   * @return array The matched filenames.
   */
  public function main($sourceFilename) {

    $info = pathinfo($sourceFilename);
    $ext = $info['extension'];
    // get basename w/o extension
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
   * The "from" attribute is not needed here, but method must exist.
   */
  public function setFrom($from) {}

     /**
   * The "from" attribute is not needed here, but method must exist.
   */
  public function setTo($to) {}

}
6.8.2 Using the Mapper

Assuming that this mapper is saved to myapp/mappers/DOSMapper.php (relative to a path on PHP's include_path or 
in PHP_CLASSPATH env variable), then you would refer to it like this in your build file:

<mapper classname="myapp.mappers.DOSMapper"/>
