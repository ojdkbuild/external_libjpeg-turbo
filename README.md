libjpeg-turbo library build for Staticlibs
==========================================

This project is a part of [Staticlibs](http://staticlibs.net/).

This project contains a CMake build file for building the [libjpeg-turbo](http://libjpeg-turbo.virtualgl.org/) library.

[libjpeg-turbo sources imported from CentOS lookaside](https://github.com/ojdkbuild/lookaside_libjpeg-turbo.git)
are used as a submodule of this project.

How to build
------------

[CMake](http://cmake.org/) is required for building.

To build the library on Windows using Visual Studio 2013 Express run the following commands using
Visual Studio development command prompt 
(`C:\Program Files (x86)\Microsoft Visual Studio 12.0\Common7\Tools\Shortcuts\VS2013 x86 Native Tools Command Prompt`):

    git clone --recursive https://github.com/staticlibs/external_libjpeg-turbo.git
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

**2017-01-04**

 * version 1.2.90-1
 * initial public version
