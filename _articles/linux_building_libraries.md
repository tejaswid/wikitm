---
title: Building Libraries Without Root Access
layout: page
thumbnail: '/assets/thumbnails/buildingBlocks_200x200.png'
blurb: 
date_published: 
categories: [Linux]
---

## Prerequisites

Before we begin, make sure you have access to a computer where you can download packages.
It is best if apt-get works on that computer. Most times, the remote machine might not have apt-get installed.
We will download source code on this computer and transfer it to the remote machine.

## Set up the workspace
Start off by creating a folder on the remote machine where you want to install the libraries. A good practice is to name it usr. I am assuming `$HOME` refers to `/home/pokemon` in this article.

```
$ cd $HOME
$ mkdir usr
$ cd usr
$ mkdir bin lib include
```

Inside usr, make three sub folders and call them bin, lib and include. They will usually be created when building the library but no harm creating them before hand. 

Some additional folders like share will be created automatically. 

Traditionally, bin contains the executables, lib contains the libraries, and include contains the headers.

Create a folder called source where we will store the source code of the libraries we download. This can be located anywhere.

```
$ cd $HOME
$ mkdir source
```

## Get the source
There are different places you can find source code for the library. Here are some options.

Method 1:
If you know the name of the library (say libPikachu), just google it and download the source from sourceforge, googlecode, git or another hosting site.

Method 2:
An alternative way is to use apt-get and download the source. Read prerequisites section.

```
$ mkdir temp
$ cd temp
$ apt-get source libPikachu
```

This will download the source code into the temp folder you just created. You will usually get a folder with the source code, a .tar.gz of the same and a .dsc of the same. Sometimes a Readme also. Keep the folder and delete the other things.

Now, transfer this folder from your computer to the source folder on the remote computer. We will build the source there.

Method 3:
If you do not know the library name but know the name of a file that is part of the library, you can look up in the ubuntu / debian packages page.

Go to [https://www.debian.org/distrib/packages#search_contents](https://www.debian.org/distrib/packages#search_contents). Choose the second option “packages that contain files named like this” and enter the name of the file. A list will appear with the name of the library. Use either of the methods above and get the source.

## Building from source
Now that we have the source, lets build it. On the remote computer,

```
$ cd $HOME
$ cd source/
$ cd libPikachu/
```

Go ahead and explore the contents. You will usually find an executable called configure inside. This is most likely the case if you followed the second method of obtaining the source. Sometimes the contents may be different. If there is a CMakeLists.txt, then simply use CMake instead of doing it this way. TODO: Figure out what must be done if there is no configure.

The configure file helps set up everything properly. We must instruct the compiler to build the source in a local folder of our choice. We do this by using the install prefix and pointing to the folder where files must be written to after build is completed.

```
$ ./configure --prefix=/home/pokemon/usr/
```

Tip: `./configure --help` should give you a list of all options possible.

If there are no errors, go ahead and build.

```
$ make
$ make install
```

This should copy files to usr/lib, usr/bin, usr/include and other folders as defined in the source.

## Things to observe
If you are interested in knowing all the files that are created by building the library, you can look up on the ubuntu / debian packages page.

The ubuntu packages page for 12.04 64bit is [http://packages.ubuntu.com/precise/amd64/](http://packages.ubuntu.com/precise/amd64/).

Choose the subcategories and navigate to the library of your choice. Then click on file list. This should give you a list of all files that are copied.

Tip: If you know the name of the library, then simply replace the name in the address below: [http://packages.ubuntu.com/precise/amd64/<library name>/filelist](http://packages.ubuntu.com/precise/amd64/<library_name>/filelist).

## Using libraries built this way
In order to use these libraries in other projects, a few settings must be modified. The compiler and the linker look in default places for libraries and includes. These are usually the /usr/lib and /usr/include. 

Since we do not have root access, we did not install our libraries to these locations. Instead we installed them to a separate folder namely /home/pokemon/usr/lib and /home/pokemon/usr/include.

Therefore, we must inform the compiler to look for libraries in these places. This is done by adding these locations to the search path. 

In a normal scenario, you would want to repeatedly use these libraries and so would want these paths to be set automatically each time you log in to the remote computer. 

This is done by adding the paths to .bashrc. TODO: I do not know the difference between editing .bash-profile, .bashrc and creating a totally new file of the type .myVar

In the home location /home/pokemon, you will usually find a hidden file named .bashrc. You can press Ctrl+H or type `ls -a` to make it visible,

Add these to the bottom of .bashrc:

```
# User specific aliases and functions
export PATH=$PATH:~/usr/bin
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:~/usr/lib:/usr/lib
export PKG_CONFIG_PATH=$PKG_CONFIG_PATH:~/usr/share/pkgconfig:~/usr/lib/pkgconfig
export C_INCLUDE_PATH=$C_INCLUDE_PATH:~/usr/include
export CPLUS_INCLUDE_PATH=$CPLUS_INCLUDE_PATH:~/usr/include
export LD_INCLUDE_PATH=$LD_INCLUDE_PATH:~/usr/include
export ACLOCAL_PATH=$ACLOCAL_PATH:~/usr/share/aclocal
```

You may not have to set all the paths that are set above. The most important ones are PATH, LD_LIBRARY_PATH and PKG_CONFIG_PATH. We are essentially concatenating the custom paths to the standard paths. 


## Troubleshooting:

Problems when doing ./configure

When configuring the source code before doing make, you might encounter a few problems. A common issue is the lack of dependencies. If you were installing the library on your computer either using apt-get or through the package manager, it would automatically install all dependencies. Since we are doing it manually, we have to install all dependencies on our own. This is a painful thing to do and there is no alternative.

The error would tell you that a particular library is missing. Simply get that library and install that first. You can google the name of that library or simply look up the error to find out what libraries are missing. It is a good practice to open the config.log to see the process of configuration. It might give you clues as to what went wrong.

Version errors

Sometimes, a particular version of a library or a higher version would be required. In this case make sure to get the required version. Build it just like any other library, using the method described above.

Especially in cases of version mismatch, you might run into another error. 

Say the version requested was v10 and you installed v20. You are very sure that you got the newer version. But the configure step still throws an error saying that v10 or higher is required. If this is the case, then you have not defined the PKG_CONFIG_PATH variable properly. 

When a library is installed, a special file with the extension .pc is created. This presence of this file is an indication that the package has been installed properly. It also contains information about the version. 

Note that not all libraries will produce this file during installation. This file is normally stored in /usr/lib/pkgconfig/. In our case, this file would be written to /home/pokemon/usr/lib/pkgconfig/.

This location must be specified in .bashrc. Some libraries decide to write this file to /home/pokemon/usr/share/pkgconfig/. Make sure you set the path accordingly.

In some cases, the configure step suggests you to edit PKG_CONFIG_PATH. It is telling you that the path is not set properly and it cannot find the .pc file.

```
$ echo $PKG_CONFIG_PATH 
```

will tell you the path that has been set. 

Missing -lLibraryName when doing make

When building the library after a successful configure step, you might encounter an error telling you that a particular library is missing. This is usually a linker error. Say you got an error telling you that -lTM is missing. This means that the linker cannot find the libTM library. 

Usually, it is looking for something like libTM.so or libTM.a. In such a case, first try to locate the library by typing

```
$ locate libTM.so
```

You should see a list with locations of similarly named libraries. It is a usual case that libraries are named as libTM.so.1 or libTM.so.1.2.0 or similar. The one with the longest name (more numbers after it) is usually the actual library file. The rest are usually symbolic links to this file.

The above error occurs because the linker is looking for libTM.so which it does not find. Instead there are linTM.so.1 and libTM.so.1.2.0 in that location. So, we need to create a symbolic link with the name libTM.so and make it point to the actual library file. Since we do not have root access, we cannot create it in the default location (usually /usr/lib or /usr/lib64). We create these links in our custom location /home/pokemon/usr/lib.

```
$ ln -s /usr/lib64/libTM.so.1.2.0 /home/pokemon/usr/lib/libTM.so
```

Now there is a file with the name that the linker is looking for that points to the actual library file. Running make should now proceed correctly. If it does not, try doing the following in the configure step:

```
$ ./configure --prefix=/home/pokemon/usr LDFLAGS=”-L/home/pokemon/usr/lib”
```

If you are using CMake instead of ./configure, the you need to add compiler flags to the CMakeLists.txt file.

```
set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} -L /home/pokemon/usr/lib“)
```

To me it is not clear why this must be done when the LD_LIBRARY_PATH variable is pointing to the location where the libraries are present. 

I think the answer can be found somewhere [here](http://www.cprogramming.com/tutorial/shared-libraries-linux-gcc.html).

This [page](http://www.ilkda.com/compile/Environment_Variables.htm) also gives a good description i think.

## Cheating

Sometimes, you dont have to do the laborious process of getting the source, copying it into the remote location and then building it. A quick work around would be to install the library on your system first. 

```
$ apt-get install libPikachu
```

Then open the corresponding page on the ubuntu packages page for this library and see the list of files that are created. Simply copy those files into similar locations on the remote computer. 

Bear in mind that you are not copying the dependencies, only files associated with this library. Also, any symbolic links you copied will be broken. Make sure to create them again. 

This is not a recommended option since several settings might be different between your and the remote computer. But it is a quick fix in times of frustration.
