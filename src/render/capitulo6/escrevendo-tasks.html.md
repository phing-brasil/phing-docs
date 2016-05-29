---
layout: page
title: Escrevendo tasks
originalLink: https://www.phing.info/docs/stable/hlhtml/index.html#d5e1862
---

6.6.1 Creating A Task

We will start creating a rather simple task which basically does nothing more than echo a message to the 
screen. See [below] for the source code and the following [below] for the XML definition that is used for 
this task.

<?php

require_once "phing/Task.php";

class MyEchoTask extends Task {

    /**
     * The message passed in the buildfile.
     */
    private $message = null;

    /**
     * The setter for the attribute "message"
     */
    public function setMessage($str) {
        $this->message = $str;
    }

    /**
     * The init method: Do init steps.
     */
    public function init() {
        // nothing to do here
    }

    /**
     * The main entry point method.
     */
    public function main() {
        print($this->message);
    }
}

?>
This code contains a rather simple, but complete Phing task. It is assumed that the file is named MyEchoTask.php. 
For this example, we're assuming that the file is placed in /home/example/classes. We'll explain the source code 
in detail shortly. But first we'd like to discuss how we should register the task to Phing so that it can be 
executed during the build process.

6.6.2 Using the Task

The task shown [above] must somehow get loaded and called by Phing. Therefore it must be made available to Phing 
so that the buildfile parser is aware a correlating XML element and it's parameters. Have a look at the minimalistic 
buildfile example given in [the buildfile below] that does exactly this.

<?xml version="1.0" ?>

<project name="test" basedir="." default="test.myecho">
    <includepath classpath="/home/example/classes" />
    <taskdef name="myecho" classname="MyEchoTask" />

    <target name="test.myecho">
      <myecho message="Hello World" />
    </target>
</project>
To register the custom task with Phing, the taskdef element (line 5) is used. See Section B.40, “TaskdefTask ” for 
a more detailed explanation. Optionally, before the taskdef element, the includepath element adds a path to PHP's 
include path. This is of course only required if the mentioned path isn't already on the include path. See 
Section B.24, “IncludePathTask ” for a more detailed explanation.

Now, as we have registered the task by assigning a name and the worker class ([see source code above]) it is ready 
for usage within the <target> context (line 8). You see that we pass the message that our task should echo to the 
screen via an XML attribute called "message".

6.6.3 Source Discussion

Now that you've got the knowledge to execute the task in a buildfile it's time to discuss how everything works.

6.6.4 Task Structure

All files containing the definition of a task class follow a common well formed structure:

Include/require statements to import all required classes

The class declaration and definition

The class's properties

The class's constructor

Setter methods for each XML attribute

The init() method

The main() method

Arbitrary private (or protected) class methods

6.6.5 Includes

Always include/require all the classes needed for this task in full written notation. Furthermore you should always
include phing/Task.php at the very top of your include block. Then include all other required system or proprietary 
classes.

6.6.6 Class Declaration

If you look at line 5 in [the source code of the task] you will find the class declaration. This will be familiar 
to you if you are experienced with OOP in PHP (we assume here that you are). Furthermore there are some fine-grained 
rules you must obey when creating the classes (see also,[naming and coding standards]):

Your classname must be exactly like the taskname you are going to implement plus the suffix "Task". In our example 
case the classname is MyEchoTask (constructed by the taskname "myecho" plus the suffix "task"). The upper/lower 
case casing is currently only for better reading. However, it is encouraged that you use it this way.

The task class you are creating must at least extend "Task" to inherit all task specific methods.

6.6.7 Class Properties

The next lines you are coding are class properties. Most of them are inherited from the Task superclass, so there's 
not need to redeclare them. Nevertheless you should declare the following ones by your own:

Taskname. Always hard code the taskname property that equals the name of the XML element that your task claims. 
Currently this information is not used - but it will be in the future.

Your arbitrary properties that reflect the XML attributes/elements which your task accepts.

In the MyEchoTask example the coded properties can be found in lines 7 to 11. Give you properties meaningful
 descriptive names that clearly state their function within the context. A couple of properties are inherited 
 from the superclass that must not be declared in the properties part of the code.

For a list of inherited properties (most of them are reserved, so be sure not to overwrite them with your own)
 can be found in the "Phing API Reference" in the docs/api/ directory.

6.6.8 The Constructor

The next block that follows is the class's constructor. It must be present and call at least the constructor 
or the parent class. Of course, you can add some initialization data here. It is recommended that you define 
your prior declared properties here.

6.6.9 Setter Methods

As you can see in the XML definition of our task ([see buildfile above], line 9) there is an attribute
 defined with the task itself, namely "message" with a value of the text string that our task should echo. 
 The task must somehow become aware of the attribute name and the value. Therefore the setter methods exist.

For each attribute you want to import to the task's namespace you have to define a method named exactly 
after the very attribute plus the string "set" prepended. This method accepts exactly one parameter that 
holds the value of the attribute. Now you can set the a class internal property to the value that is 
passed via the setter method.

In the setter method you should also perform any casting operations and/or check if the attribute value is a 
valid value. If this is not the case, throw a BuildException. In some cases, such as when you have three 
attributes and at least one of them should be set, you may want to check the attribute values inside the 
init() or main() method.

In out example the setter is named setMessage, because the XML attribute the echo task accepts is "message". 
setMessage now takes the string "Hello World" provided by the parser and sets the value of the internal class
 property $strMessage to "Hello World". It is now available to the task for further disposal.

6.6.10 Creator Methods

Creator methods allow you to manage nested XML tags in your new Phing Task.

6.6.11 init() Method

The init method gets called when the <taskname> xml element closes. It must be implemented even if it does 
nothing like in the example above. You can do init steps here required to setup your task object properly. 
After calling the Init-Method the task object remains untouched by the parser. Init should not perform 
operations related somehow to the action the task performs. An example of using init may be cleaning up the 
$strMessage variable in our example (i.e. trim($strMessage)) or importing additional workers needed for this task.

The init method should return true or an error object evaluated by the governing logic. If you don't implement 
init method, phing will shout down with a fatal error.

6.6.12 main() Method

There is exactly one entry point to execute the task. It is called after the complete buildfile has been parsed 
and all targets and tasks have been scheduled for execution. From this point forward the very implementation of 
the tasks action starts. In case of our example a message (imported by the proper setter method) is Logged to 
the screen through the system's "Logger" service (the very action this task is written for). The Log() method-call 
in this case accepts two parameters: a event constant and the message to log.

6.6.13 Arbitrary Methods

For the more or less simple cases (as our example) all the logic of the task is coded in the Main() method. 
However for more complex tasks common sense dictates that particular action should be swapped to smaller, 
logically contained units of code. The most common way to do this is separating logic into private class methods
 - and in even more complex tasks in separate libraries.

private function myPrivateMethod() {
    // definition
}