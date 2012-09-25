Gflags 2.0
==========

This repository contains a C++ version of the Google commandline flags module.
Documentation for the C++ implementation is in doc/.


Origin
------

The code was taken from https://code.google.com/p/gflags/ and adapted
to work with cmake properly.


Installation
------------

For building in a Unix environment, simply type

    cmake -G 'Unix Makefiles' && make && make install

On windows, open a command prompt and enter

    cmake -G 'Visual Studio 10'

This will generate project files for Visual Studio 2010.

Of course you can use any other CMake generator you like.
