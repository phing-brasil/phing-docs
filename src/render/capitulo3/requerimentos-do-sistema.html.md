---
layout: page
title: Requerimentos do sistema
originalLink: https://www.phing.info/docs/stable/hlhtml/index.html#d5e597
---

Para utilizar o Phing você precisa ter instalado o PHP 5.2 ou acima compilado
com **--with-libxml2** e **--with-xsl** se você quiser utilizar funcionalidades
avançadas

## Sistema operacional

Desenhado para portabilidade, Phing roda em todas as plataformas que rodam PHP.
Entretanto algumas funcionalidades avançadas poderam não funcionar normalmente ou
serão simplesmente ignoradas em algumas plataformas (como por exemplo chmod na 
plataforma Windows)

Para evitar problemas com o Phing, é recomendado utilizar uma plataforma baseada
em Unix. Como por exemplo Linux, FreeBSD, OpenBSD, etc.

## Dependências de software

|Software|Necessário para|Fonte|
|--------|---------------|-----|
|PHP 5.2+|Execução|http://www.php.net|
|PHPUnit 3.6.0+	|Opcional; habilita tarefas adicionais|	http://www.phpunit.de|
|Xdebug 2.0.5+	|Opcional; habilita tarefas adicionais|	http://www.xdebug.org|
|SimpleTest 1.0.1 |beta+	Opcional; habilita tarefas adicionais|	http://simpletest.sourceforge.net|
|phpDocumentor 2.0.0b7+ (PEAR package)|Opcional; habilita tarefas adicionais|http://pear.phpdoc.org|
|VersionControl_SVN (PEAR package)|Opcional; habilita tarefas adicionais|http://pear.php.net/package/VersionControl_SVN|
|VersionControl_Git (PEAR package)|Opcional; habilita tarefas adicionais|http://pear.php.net/package/VersionControl_Git|
|PHP_CodeSniffer (PEAR package)|Opcional; habilita tarefas adicionais|http://pear.php.net/package/PHP_CodeSniffer|
|Archive_Tar (PEAR package)|Opcional; habilita tarefas adicionais|http://pear.php.net/package/Archive_Tar|
|Services_Amazon_S3 (PEAR package)|Opcional; habilita tarefas adicionais|http://pear.php.net/package/Services_Amazon_S3|
|HTTP_Request2 (PEAR package)|Opcional; habilita tarefas adicionais|http://pear.php.net/package/HTTP_Request2|
|PHP Depend|Opcional; habilita tarefas adicionais|http://www.pdepend.org|
|PHP Mess Detector|Opcional; habilita tarefas adicionais|http://www.phpmd.org|
|PHP Copy/Paste Detector|Opcional; habilita tarefas adicionais|http://pear.phpunit.de|

## Atenção

Phing não funciona com o modo seguro habilitado no PHP!