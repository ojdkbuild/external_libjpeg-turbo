libjpeg-turbo library build for Staticlibs
==========================================

[![travis](https://travis-ci.org/staticlibs/external_libjpeg-turbo.svg?branch=master)](https://travis-ci.org/staticlibs/external_libjpeg-turbo)
[![appveyor](https://ci.appveyor.com/api/projects/status/github/staticlibs/external_libjpeg-turbo?svg=true)](https://ci.appveyor.com/project/staticlibs/external-libjpeg-turbo)

This project is a part of [Staticlibs](http://staticlibs.net/).

This project contains a CMake build file for building the [libjpeg-turbo](http://libjpeg-turbo.virtualgl.org/) library that
can be used to build sources imported from [CentOS lookaside](https://github.com/staticlibs/lookaside_libjpeg-turbo.git).

How to build
------------

[CMake](http://cmake.org/) is required for building.

[NASM](http://nasm.us/) is also required for building on Windows.
You can obtain ready-to-use NASM distribution from 
[tools_windows_nasm](https://github.com/staticlibs/tools_windows_nasm) repository.

To build the library on Windows using Visual Studio 2013 Express run the following commands using
Visual Studio development command prompt 
(`C:\Program Files (x86)\Microsoft Visual Studio 12.0\Common7\Tools\Shortcuts\VS2013 x86 Native Tools Command Prompt`):

    git clone https://github.com/staticlibs/lookaside_libjpeg-turbo.git
    git clone https://github.com/staticlibs/external_libjpeg-turbo.git
    cd external_libjpeg-turbo
    mkdir build
    cd build
    cmake ..
    msbuild external_libjpeg-turbo.sln

See [StaticlibsToolchains](https://github.com/staticlibs/wiki/wiki/StaticlibsToolchains) for 
more information about the CMake toolchains setup and cross-compilation.

License information
-------------------

This project is released under the [Apache License 2.0](http://www.apache.org/licenses/LICENSE-2.0).

Changelog
---------

**2019-10-02**

 * version 1.2.90-4
 * 4 patches applied

**2019-01-07**

 * version 1.2.90-3
 * lookaside sources minor update

**2018-03-20**

 * version 1.2.90-2
 * drop submodule
 * linux, macos and android builds

**2017-01-04**

 * version 1.2.90-1
 * initial public version
