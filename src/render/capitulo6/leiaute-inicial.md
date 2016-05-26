---
layout: page
title: Tipos básicos
originalLink: https://www.phing.info/docs/stable/hlhtml/index.html#d5e1706
---

6.2.1 Files And Directories

Before you are going to start to extend Phing let's have a look at the source layout. You should be comfortable with the organization of files witch in the source tree of Phing before start coding. After you extracted the source distribution or checked it out from git you should see the following directory structure:

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

The following table briefly describes the contents of the major directories:

Table 6.1: Phing source tree directories

Directory	Contents
bin

The basic applications (phing, configure) as well as the wrapper scripts for different platforms (currently Unix and Windows).

classes

Repository of all the classes used by Phing. This is the base directory that should be on the PHP include_path. In this directory you will find the subdirectory phing/ with all the Phing relevant classes.

docs

Documentation files. Generated books, online manuals as well as the PHPDoc generated API documentation.

test

A set of testcases for different tasks, mappers and types. If you are developing in git you should add a testcase for each implementation you check in.


Currently there is no distinction between the source layout and the build layout of Phing. The directory layout shows the file tree that carries some additional files like the Phing website. Later on there may be a buildfile to create a clean distribution tree of Phing itself.

6.2.2 File Naming Conventions

There are some file naming conventions used by Phing. Here's a quick rundown on the most basic conventions. A more detailed list can be found in [See Naming And Coding Standards]:

Filenames consist of no more or less than two elements: name and extension .

Choose short descriptive filenames (must be less than 31 chars)

Names must not contain dots.

Files containing PHP code must end with the extension .php .

There must be only one class per file (no procedural methods allowed, use a separate file for them), with the exception of "inner"-type / helper classes that can be declared in the same file as the "outer" / main class.

The name portion of the file must be named exactly like the class it contains.

Buildfiles and configure rulesets must end with the extension .xml .

6.2.3 Coding Standards

We are using PEAR coding standards. We are using a less strict version of these standards, but we do insist that new contributions have phpdoc comments and make explicitly declarations about public/protected/private variables and methods. If you have suggestions about improvements to Phing codebase, don't hesitate to let us know.