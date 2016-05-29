---
layout: page
title: Ciclo de vida do build
originalLink: https://www.phing.info/docs/stable/hlhtml/index.html#d5e1824
---

This section exists to explain -- or try -- how Phing "works". Particularly, how Phing proceeds through a 
build file and invokes tasks and types based on the tags that it encounters.

6.5.1 How Phing Parses Buildfiles

Phing uses an ExpatParser class and PHP's native expat XML functions to handle the parsing of build files. 
The handler classes all extend the phing.parser.AbstractHandler class. These handler classes "handle" the 
tags that are found in the buildfile.

Core tasks and datatypes are mapped to XML tag names in the defaults.properties files -- specifically 
phing/tasks/defaults.properties and phing/types/defaults.properties.

It works roughly like this:

phing.parser.RootHandler is registered to handle the buildfile XML document

RootHanlder expects to find exactly one element: <project>. RootHandler invokes the ProjectHandler with the 
attributes from the <project> tag or throws an exception if no <project> is found, or if something else is 
found instead.

ProjectHandler expects to find <target> tags; for these ProjectHandler invokes the TargetHandler. ProjectHandler 
also has exceptions for handling certain tasks that can be performed at the top-level: <resolve>, <taskdef>, 
<typedef>, and <property>; for these ProjectHandler invokes the TaskHandler class. If a tag is presented that 
doesn't match any expected tags, then ProjectHandler assumes it is a datatype and invokes the DataTypeHandler.

TargetHandler expects all tags to be either tasks or datatypes and invokes the appropriate handler (based on 
the mappings provided in the defaults.properties files).

Tasks and datatypes can have nested elements, but only if they correspond to a create*() method in the task or 
datatype class. E.g. a nested <param> tag must correspond to a createParam() method of the task or datatype.