
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

Executables and libraries are defined using the `add_executable()` and `add_library()` commands. The resulting binary files have appropriate prefixes, suffixes and extensions for the platform targeted. Dependencies between binary targets are expressed using the `target_link_libraries()` command:

``` c
add_library(archive archive.cpp zip.cpp lzma.cpp)
add_executable(zipapp zipapp.cpp)
target_link_libraries(zipapp archive) // target zipapp depends on archive for linking
```

1. archive is defined as a static library – an archive containing objects compiled from archive.cpp, zip.cpp, and lzma.cpp. 
2. zipapp is defined as an executable formed by compiling and linking zipapp.cpp. When linking the zipapp executable, the archive static library is linked in.