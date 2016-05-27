---
layout: page
title: Tipos básicos
originalLink: https://www.phing.info/docs/stable/hlhtml/index.html#d5e1778
---

PHP installations are typically quite customized -- e.g. different memory_limit, execution timeout values, etc. The first thing that Phing does is modify PHP INI variables to create a standard PHP environment. This is performed by the init layer of Phing that uses a three-level initialization procedure. It basically consists of three different files:

Platform specific wrapper scripts in bin/

Main application in bin/

Phing class in classes/phing/

At the first look this may seem to be unnecessary overhead. Why three levels of initialization? The main reason why there are several entry points is that Phing is build so that other frontends (e.g. PHP-GTK) could be used in place of the command line.

6.3.1 Wrapper Scripts

This scripts are technical not required but provided for the ease of use. Imagine you have to type every time you want to build your project:

php -qC /path/to/phing/bin/phing.php -verbose all distro snapshot
Indeed that is not very elegant. Furthermore if you are lax in setting your environment variables these script can guess the proper variables for you. However you should always set them.

The scripts are platform dependent, so you will find shell scripts for Unix like platforms (sh) as well as the batch scripts for Windows platforms. If you set-up your path properly you can call Phing everywhere in your system with this command-line (referring to the above example):

phing -v2 all distro
6.3.2 The Main Application (phing.php)

This is basically a wrapper for the Phing class that actually does all the logic for you. If you look at the source code for phing.php you will see that all real initialization is handled in the Phing class. phing.php is simply the command line entry point for Phing.

6.3.3 The Phing Class

Given that all the prior initialization steps passed successfully the Phing is included and Phing::startup() is invoked by the main application script. It sets-up the system components, system constants ini-settings, PEAR and some other stuff. The detailed start-up process is as follows:

Start Timer

Set System Constants

Set Ini-Settings

Set Include Paths

After the main application completed all operations (successfully or unsuccessfully) it calls Phing::shutdown(EXIT_CODE) that takes care of a proper destruction of all objects and a gracefully termination of the program by returning an exit code for shell usage (see [See Program Exit Codes] for a list of exit codes).