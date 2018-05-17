
### CMAKE is a makefile/solution generator

CMAKE generates platforms specific projects e.g. .sln for MS, Makefile for unix, .xcodeproj for macs etc.
One can list available generators on system using:
`cmake --help`

How they work internally (how do they choose default generator for given platform)?
e.g. on windows:
It appears that CMake looks at the Windows Registry to determine which generator to use. It searches the Visual Studio registry subkeys (6.0, 7.0, etc) in [HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\VisualStudio\\ for an entry called InstallDir. If one is found, it uses the corresponding generator. (It will use the newest version of Visual Studio available.) Otherwise, it uses the NMake generator.


### Why is `cmake ..` used frequently from a build dir?

(from stackoverflow)
 When you call cmake [path], you ask it to generate a Makefile in the current directory following instructions given in [path]/CMakeLists.txt

Usually cmake output some messages while it is working, and after it is done without errors, you can type "make" to execute your newly created Makefile.

CMakeLists.txt files can reference other CMakeLists.txt file in sub-directories, so you are usually only interested by the CMakeLists.txt of the top directory, not the other ones.

Using an empty "build" directory is a technique called "out-of-source build", in which all your generated files (.o, executable, Makefile, .anything) are generated in the separate "build" directory and not mixed with source files. If you want to clean all, you can delete all the content of the build directory.

In fact, you can put your "build" directory in any place, as long as you give cmake the correct path of the top CMakeLists.txt. You can even have several build directories. It is very useful if you need several different builds at the same time (with different options, different versions of gcc, etc.)

### `Cmake ..` vs `cmake --build .`

cmake takes in Cmakelists.txt and populates given directory with the system makefiles.
cmake --build runs those make files to fulfill targets

### CMAKE directories

When CMake processes a project source tree, the entry point is a source file called `CMakeLists.txt` in the top-level source directory. This file may contain the entire build specification or use the `add_subdirectory()` command to add subdirectories to the build. Each subdirectory added by the command must also contain a CMakeLists.txt file as the entry point to that directory. For each source directory whose CMakeLists.txt file is processed CMake generates a corresponding directory in the build tree to act as the default working and output directory.

### Binary targets

In general, targets comprise executables or libraries which are defined by calling add_executable or  add_library and which can have many properties set.

They can have dependencies on one another, which for targets such as these just means that dependent ones will be built after their dependencies.

Executables and libraries are defined using the `add_executable()` and `add_library()` commands. The resulting binary files have appropriate prefixes, suffixes and extensions for the platform targeted. Dependencies between binary targets are expressed using the `target_link_libraries()` command:

``` cmake
add_library(archive archive.cpp zip.cpp lzma.cpp)
add_executable(zipapp zipapp.cpp)
target_link_libraries(zipapp archive) // target executable zipapp depends on library archive for linking
```

1. archive is defined as a static library – an archive containing objects compiled from archive.cpp, zip.cpp, and lzma.cpp. 
2. zipapp is defined as an executable formed by compiling and linking zipapp.cpp. When linking the zipapp executable, the archive static library is linked in.

### Logging useful variables

```cmake
# ------------------------- Begin Generic CMake Variable Logging ------------------

# /*    C++ comment style not allowed   */


# if you are building in-source, this is the same as CMAKE_SOURCE_DIR, otherwise
# this is the top level directory of your build tree
MESSAGE( STATUS "CMAKE_BINARY_DIR:         " ${CMAKE_BINARY_DIR} )

# if you are building in-source, this is the same as CMAKE_CURRENT_SOURCE_DIR, otherwise this
# is the directory where the compiled or generated files from the current CMakeLists.txt will go to
MESSAGE( STATUS "CMAKE_CURRENT_BINARY_DIR: " ${CMAKE_CURRENT_BINARY_DIR} )

# this is the directory, from which cmake was started, i.e. the top level source directory
MESSAGE( STATUS "CMAKE_SOURCE_DIR:         " ${CMAKE_SOURCE_DIR} )

# this is the directory where the currently processed CMakeLists.txt is located in
MESSAGE( STATUS "CMAKE_CURRENT_SOURCE_DIR: " ${CMAKE_CURRENT_SOURCE_DIR} )

# contains the full path to the top level directory of your build tree
MESSAGE( STATUS "PROJECT_BINARY_DIR: " ${PROJECT_BINARY_DIR} )

# contains the full path to the root of your project source directory,
# i.e. to the nearest directory where CMakeLists.txt contains the PROJECT() command
MESSAGE( STATUS "PROJECT_SOURCE_DIR: " ${PROJECT_SOURCE_DIR} )

# set this variable to specify a common place where CMake should put all executable files
# (instead of CMAKE_CURRENT_BINARY_DIR)
MESSAGE( STATUS "EXECUTABLE_OUTPUT_PATH: " ${EXECUTABLE_OUTPUT_PATH} )

# set this variable to specify a common place where CMake should put all libraries
# (instead of CMAKE_CURRENT_BINARY_DIR)
MESSAGE( STATUS "LIBRARY_OUTPUT_PATH:     " ${LIBRARY_OUTPUT_PATH} )

# tell CMake to search first in directories listed in CMAKE_MODULE_PATH
# when you use FIND_PACKAGE() or INCLUDE()
MESSAGE( STATUS "CMAKE_MODULE_PATH: " ${CMAKE_MODULE_PATH} )

# this is the complete path of the cmake which runs currently (e.g. /usr/local/bin/cmake)
MESSAGE( STATUS "CMAKE_COMMAND: " ${CMAKE_COMMAND} )

# this is the CMake installation directory
MESSAGE( STATUS "CMAKE_ROOT: " ${CMAKE_ROOT} )

# this is the filename including the complete path of the file where this variable is used.
MESSAGE( STATUS "CMAKE_CURRENT_LIST_FILE: " ${CMAKE_CURRENT_LIST_FILE} )

# this is linenumber where the variable is used
MESSAGE( STATUS "CMAKE_CURRENT_LIST_LINE: " ${CMAKE_CURRENT_LIST_LINE} )

# this is used when searching for include files e.g. using the FIND_PATH() command.
MESSAGE( STATUS "CMAKE_INCLUDE_PATH: " ${CMAKE_INCLUDE_PATH} )

# this is used when searching for libraries e.g. using the FIND_LIBRARY() command.
MESSAGE( STATUS "CMAKE_LIBRARY_PATH: " ${CMAKE_LIBRARY_PATH} )

# the complete system name, e.g. "Linux-2.4.22", "FreeBSD-5.4-RELEASE" or "Windows 5.1"
MESSAGE( STATUS "CMAKE_SYSTEM: " ${CMAKE_SYSTEM} )

# the short system name, e.g. "Linux", "FreeBSD" or "Windows"
MESSAGE( STATUS "CMAKE_SYSTEM_NAME: " ${CMAKE_SYSTEM_NAME} )

# only the version part of CMAKE_SYSTEM
MESSAGE( STATUS "CMAKE_SYSTEM_VERSION: " ${CMAKE_SYSTEM_VERSION} )

# the processor name (e.g. "Intel(R) Pentium(R) M processor 2.00GHz")
MESSAGE( STATUS "CMAKE_SYSTEM_PROCESSOR: " ${CMAKE_SYSTEM_PROCESSOR} )

# is TRUE on all UNIX-like OS's, including Apple OS X and CygWin
MESSAGE( STATUS "UNIX: " ${UNIX} )

# is TRUE on Windows, including CygWin
MESSAGE( STATUS "WIN32: " ${WIN32} )

# is TRUE on Apple OS X
MESSAGE( STATUS "APPLE: " ${APPLE} )

# is TRUE when using the MinGW compiler in Windows
MESSAGE( STATUS "MINGW: " ${MINGW} )

# is TRUE on Windows when using the CygWin version of cmake
MESSAGE( STATUS "CYGWIN: " ${CYGWIN} )

# is TRUE on Windows when using a Borland compiler
MESSAGE( STATUS "BORLAND: " ${BORLAND} )

# Microsoft compiler
MESSAGE( STATUS "MSVC: " ${MSVC} )
MESSAGE( STATUS "MSVC_IDE: " ${MSVC_IDE} )
MESSAGE( STATUS "MSVC60: " ${MSVC60} )
MESSAGE( STATUS "MSVC70: " ${MSVC70} )
MESSAGE( STATUS "MSVC71: " ${MSVC71} )
MESSAGE( STATUS "MSVC80: " ${MSVC80} )
MESSAGE( STATUS "CMAKE_COMPILER_2005: " ${CMAKE_COMPILER_2005} )


# set this to true if you don't want to rebuild the object files if the rules have changed,
# but not the actual source files or headers (e.g. if you changed the some compiler switches)
MESSAGE( STATUS "CMAKE_SKIP_RULE_DEPENDENCY: " ${CMAKE_SKIP_RULE_DEPENDENCY} )

# since CMake 2.1 the install rule depends on all, i.e. everything will be built before installing.
# If you don't like this, set this one to true.
MESSAGE( STATUS "CMAKE_SKIP_INSTALL_ALL_DEPENDENCY: " ${CMAKE_SKIP_INSTALL_ALL_DEPENDENCY} )

# If set, runtime paths are not added when using shared libraries. Default it is set to OFF
MESSAGE( STATUS "CMAKE_SKIP_RPATH: " ${CMAKE_SKIP_RPATH} )

# set this to true if you are using makefiles and want to see the full compile and link
# commands instead of only the shortened ones
MESSAGE( STATUS "CMAKE_VERBOSE_MAKEFILE: " ${CMAKE_VERBOSE_MAKEFILE} )

# this will cause CMake to not put in the rules that re-run CMake. This might be useful if
# you want to use the generated build files on another machine.
MESSAGE( STATUS "CMAKE_SUPPRESS_REGENERATION: " ${CMAKE_SUPPRESS_REGENERATION} )


# A simple way to get switches to the compiler is to use ADD_DEFINITIONS().
# But there are also two variables exactly for this purpose:

# the compiler flags for compiling C sources
MESSAGE( STATUS "CMAKE_C_FLAGS: " ${CMAKE_C_FLAGS} )

# the compiler flags for compiling C++ sources
MESSAGE( STATUS "CMAKE_CXX_FLAGS: " ${CMAKE_CXX_FLAGS} )


# Choose the type of build.  Example: SET(CMAKE_BUILD_TYPE Debug)
MESSAGE( STATUS "CMAKE_BUILD_TYPE: " ${CMAKE_BUILD_TYPE} )

# if this is set to ON, then all libraries are built as shared libraries by default.
MESSAGE( STATUS "BUILD_SHARED_LIBS: " ${BUILD_SHARED_LIBS} )

# the compiler used for C files
MESSAGE( STATUS "CMAKE_C_COMPILER: " ${CMAKE_C_COMPILER} )

# the compiler used for C++ files
MESSAGE( STATUS "CMAKE_CXX_COMPILER: " ${CMAKE_CXX_COMPILER} )

# if the compiler is a variant of gcc, this should be set to 1
MESSAGE( STATUS "CMAKE_COMPILER_IS_GNUCC: " ${CMAKE_COMPILER_IS_GNUCC} )

# if the compiler is a variant of g++, this should be set to 1
MESSAGE( STATUS "CMAKE_COMPILER_IS_GNUCXX : " ${CMAKE_COMPILER_IS_GNUCXX} )

# the tools for creating libraries
MESSAGE( STATUS "CMAKE_AR: " ${CMAKE_AR} )
MESSAGE( STATUS "CMAKE_RANLIB: " ${CMAKE_RANLIB} )

#
#MESSAGE( STATUS ": " ${} )

# ------------------------- End of Generic CMake Variable Logging ------------------

```


### dependency management via find_XXX

#### find_package

More at https://gitlab.kitware.com/cmake/community/wikis/doc/tutorials/How-To-Find-Libraries

The
find_package()
command will look in the module path for Find.cmake, which is the
typical way for finding libraries. First CMake checks all directories in
${CMAKE_MODULE_PATH}, then it looks in its own module directory
/share/cmake-x.y/Modules/.

If no such file is found, it looks for Config.cmake or
-config.cmake, which are supposed to be installed by
libraries (but there are currently not yet many libraries which install
them) and that don't do detection, but rather just contain hardcoded
values for the installed library.


Command find_package has two modes: Module mode and Config mode. You are trying to use Module mode when you actually need Config mode.

Module mode
Find<package>.cmake file located within your project. Something like this:

CMakeLists.txt
cmake/FindFoo.cmake
cmake/FindBoo.cmake
CMakeLists.txt content:

list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_LIST_DIR}/cmake")
find_package(Foo REQUIRED) # FOO_INCLUDE_DIR, FOO_LIBRARIES
find_package(Boo REQUIRED) # BOO_INCLUDE_DIR, BOO_LIBRARIES

include_directories("${FOO_INCLUDE_DIR}")
include_directories("${BOO_INCLUDE_DIR}")
add_executable(Bar Bar.hpp Bar.cpp)
target_link_libraries(Bar ${FOO_LIBRARIES} ${BOO_LIBRARIES})
Note that CMAKE_MODULE_PATH has high priority and may be usefull when you need to rewrite standard Find<package>.cmake file.

Config mode (install)
<package>Config.cmake file located outside and produced by install command of other project (Foo for example).

foo library:
```
> cat CMakeLists.txt 
cmake_minimum_required(VERSION 2.8)
project(Foo)

add_library(foo Foo.hpp Foo.cpp)
install(FILES Foo.hpp DESTINATION include)
install(TARGETS foo DESTINATION lib)
install(FILES FooConfig.cmake DESTINATION lib/cmake/Foo)
```
Simplified version of config file:
```
> cat FooConfig.cmake 
add_library(foo STATIC IMPORTED)
find_library(FOO_LIBRARY_PATH foo HINTS "${CMAKE_CURRENT_LIST_DIR}/../../")
set_target_properties(foo PROPERTIES IMPORTED_LOCATION "${FOO_LIBRARY_PATH}")
```
By default project installed in CMAKE_INSTALL_PREFIX directory:
```
> cmake -H. -B_builds
> cmake --build _builds --target install
-- Install configuration: ""
-- Installing: /usr/local/include/Foo.hpp
-- Installing: /usr/local/lib/libfoo.a
-- Installing: /usr/local/lib/cmake/Foo/FooConfig.cmake
```
Config mode (use)
Use find_package(... CONFIG) to include FooConfig.cmake with imported target foo:
```
> cat CMakeLists.txt 
cmake_minimum_required(VERSION 2.8)
project(Boo)

# import library target `foo`
find_package(Foo CONFIG REQUIRED)

add_executable(boo Boo.cpp Boo.hpp)
target_link_libraries(boo foo)
> cmake -H. -B_builds -DCMAKE_VERBOSE_MAKEFILE=ON
> cmake --build _builds
Linking CXX executable Boo
/usr/bin/c++ ... -o Boo /usr/local/lib/libfoo.a
```
Note that imported target is highly configurable. See my answer

