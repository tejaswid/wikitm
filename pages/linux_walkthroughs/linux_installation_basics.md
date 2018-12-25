---
title: Installation Basics in Linux
keywords: linux, installation, basics
summary: "To explain the installation process in Linux"
sidebar: linux_walkthroughs_sidebar
permalink: linux_installation_basics.html
folder: linux_walkthroughs
---

## Overview

This  tutorial is aimed at those who have just started using Linux. Generally when users from the Windows background enter the Linux scene, they are totally stumped by the software installation method. They were used to the luxury of double clicking on a single file and getting their software installed. But now they have to type cryptic commands to do the same.

Generally you would get Linux software in the tarball format (.tgz) This file has to be uncompressed into any directory using tar command. In case you download a new tarball by the name game.tgz, then you would have to type the following command. You can also do this by right-click -> extract on some modern versions of Linux.

```
$ tar xfvz game.tgz
```

This would create a directory within the current directory and extract all the files into that new directory. Once this is complete the installation instructions ask you to execute the 3 (now famous) commands : configure, make & make install. Most of the users do this and successfully install their software. But most of the newbies have no idea what this really does. The rest of the article shall explain the meaning of these 3 commands.

Each software comes with a few files which are solely for the sake of installation. One of them is the configure script. The user has to run the following command at the prompt.

```
$ ./configure
```

The above command makes the shell run the script named 'configure' which exists in the current directory. The configure script basically consists of many lines which are used to check some details about the machine on which the software is going to be installed. This script checks for lots of dependencies on your system. For the particular software to work properly, it may require a lot of things to be present on your machine already. When you run the configure script you would see a lot of output on the screen , each being some sort of question and a respective yes/no as the reply. If any of the major requirements are missing on your system, the configure script would exit and you cannot proceed with the installation, until you get those required things. 

The main job of the configure script is to create a ' Makefile ' . This is a very important file for the installation process. Depending on the results of the tests (checks) that the configure script performed it would write down the various steps that need to be taken (while compiling the software) in the file named Makefile.

If you get no errors and the configure script runs successfully (if there is any error the last few lines of the output would glaringly state the error) then you can proceed with the next command which is

```
$ make
```

' make ' is actually a utility which exists on almost all Unix systems. For make utility to work it requires a file named Makefile in the same directory in which you run make. As we have seen, the configure script's main job was to create a file named Makefile to be used with make utility. (Sometimes the Makefile is named as makefile also)

make would use the directions present in the Makefile and proceed with the installation. The Makefile indicates the sequence, that Linux must follow to build various components / sub-programs of your software. The sequence depends on the way the software is designed as well as many other factors.

The Makefile actually has a lot of labels (sort of names for different sections). Hence depending on what needs to be done the control would be passed to the different sections within the Makefile. It is possible that at the end of one of the section there is a command to go to some next section.

Basically the make utility compiles all your program code and creates the executables. For particular section of the program to complete might require some other part of the code already ready, this is what the Makefile does. It sets the sequence for the events so that your program does not complain about missing dependencies.

One of the labels present in the Makefile happens to be named 'install'.

If make ran successfully then you are almost done with the installation. Only the last step remains which is

As indicated before make uses the file named Makefile in the same directory. When you run make without any parameters, the instructions in the Makefile begin executing from the start and as per the rules defined within the Makefile (particular sections of the code may execute after one another. That's why labels are used to jump from one section to another). But when you run make with install as the parameter, the make utility searches for a label named install within the Makefile, and executes only that section of the Makefile.

The install section happens to be only a part where the executables and other required files created during the previous step (i.e. make) are copied into the required final directories on your machine. E.g. the executable that the user runs may be copied to the /usr/local/bin so that all users are able to run the software. Similarly all the other files are also copied to the standard directories in Linux (e.g. /usr/local/lib and /usr/local/include). Remember that when you ran make, all the executables were created in the temporary directory where you had unzipped your original tarball. So when you run make install, these executables are copied to the final directories.

Thats it !! Now the installation process must be clear to you. You surely will feel more at home when you begin your next software installation.

{% include links.html %}
