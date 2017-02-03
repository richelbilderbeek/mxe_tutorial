# The basic build

This basic build consists of a 'Hello World' program, written in C++98. 
It uses the Qt Creator default settings: Qt Creator will create a Qt Creator project file, which in turn will use GCC.

 * What is a C++98 'Hello world' program?
 * The Travis build file
 * The build script
 * The Qt Creator project file
 * The source file

This basic build uses the code of
[https://github.com/richelbilderbeek/travis_qmake_gcc_cpp98](https://github.com/richelbilderbeek/travis_qmake_gcc_cpp98).

## What is a C++98 'Hello world' program?

A 'Hello World' program shows the text 'Hello world' on the screen. 
It is a minimal program. 
Its purpose is to show that all machinery is in place to create an executable from C++ source code. 

### `main.cpp`

```
#include <iostream>

int main() 
{
  std::cout << "Hello world\n";
}
```

 * `#include <iostream>`: read a header file called 'iostream'
 * `int main() { /* your code */ }`: the `main` function is the starting point of a C++ program. Its body is between curly braces
 * `std::cout << "Hello world\n";`: show the text 'Hello world' on screen and go to the next line

All the code does is display the text 'Hello world',
which is a traditional start for many programming languages. 
The code is written in C++98. 
It does not use features from the newer C++ standards, 
but can be compiled under these newer standards. It will not compile under plain C.

## The Travis file

Travis CI is set up by a file called `.travis.yml`. 
The filename starts with a dot, which means it is a hidden file on UNIX systems. 
The extension `yml` is an abbreviation of 'Yet another Markup Language'Yet Another Markup Language.
The '.travis.yml' file to build and run a 'Hello world' program looks like this:

```
language: cpp
compiler: gcc

script: 
  - qmake
  - make
  - ./travis_qmake_gcc_cpp98
```

This .travis.yml file has the following elements:

 * `language: cpp`: The main programming language of this project is C++
 * `compiler: gcc`: the C++ code will be compiled by the GCC
 * `script:`: The script that Travis will run. In this case, it will:
  * `qmake`: build a makefile
  * `make`: make the executable
  * `./travis_qmake_gcc_cpp98`: run the executable

This build script can fail in in two places:

 * The bash script can fail
 * The executable can return an error code. A 'Hello World' program is intended to return 
   the error code for 'everything went fine'. Other programs in this tutorial return error codes depending on test cases. 
   It may also be that dynamically linked libraries cannot be found, which crashes the program at startup

## Qt Creator project file

The following Qt Creator project file is used in this 'Hello world' build:

```
SOURCES += main.cpp
QMAKE_CXXFLAGS += -Wall -Wextra -Wshadow -Wnon-virtual-dtor -pedantic -Weffc++ -Werror
```

This Qt Creator project file has the following elements:

 * `SOURCES += main.cpp`: the file 'main.cpp' is a source file, that has to be compiled
 * `QMAKE_CXXFLAGS += -Wall -Wextra -Wshadow -Wnon-virtual-dtor -pedantic -Weffc++ -Werror`: the 
    project is checked with all warnings (`-Wall`), with extra warnings (`-Wextra`), disallowing the
    shadowing of parameters (`-Wshadow`), warning for non-virtual destructors in virtual base classes
    `-Wnon-virtual-dtor`, some other pedantic warnings (`-Wpedantic`), with the 'Effective C++' 
    advices (`-Weffc++`) enforced. A warning is treated as an error (`-Werror`). 
    This forces you (and your collaborators) to write tidy code.

## Cross compiling

The cross compiling script looks like this:

```
#!/bin/bash
cd travis_qmake_gcc_cpp98

# Remove all warnings
sed 's/^QMAKE_CXXFLAGS/# QMAKE_CXXFLAGS/' travis_qmake_gcc_cpp98.pro

i686-w64-mingw32.static-qmake-qt4

make

cd ..

if [ ! -f ./travis_qmake_gcc_cpp98/release/travis_qmake_gcc_cpp98.exe ]
then
  echo "Error: Windows executable has not been created"
  exit 1
fi
```

It has the following elements:

 * `#!/bin/bash`: the shebang, indicating this is a bash script
 * `cd travis_qmake_gcc_cpp98`: go into the folder `travis_qmake_gcc_cpp98`
 * `# Remove all warnings`: a comment. All comments start with a pound (`#`)
 * `sed 's/^QMAKE_CXXFLAGS/# QMAKE_CXXFLAGS/' travis_qmake_gcc_cpp98.pro`: comment out all lines
   that start with `QMAKE_CXXFLAGS`. This to remove all the warnings
 * `i686-w64-mingw32.static-qmake-qt5`: create a makefile for the only `.pro` file in that folder using `Qt5`'s `qmake`
 * `make`: build the executable
 * `cd ..`: go up one folder

```
if [ ! -f ./travis_qmake_gcc_cpp98/release/travis_qmake_gcc_cpp98.exe ]
then
  echo "Error: Windows executable has not been created"
fi
```

 * if the executable file does not exist, then show an error message and exit the script with an error code

The executable is put into a folder called `release`. You may not have expected that: usually code
ends up in a `debug` folder. Then, remember that you *cross compile*: your debugger cannot handle
a Windows executable.

The project file used for this cross-compile is rather strict: all warnings are turned on,
and warnings are escalated to errors. This may be inappropriate for cross-compiling: 
your code *has* been tested on your favorite platform (if not, you should), 
and you may choose to ignore warnings. This script comments out all such warnings.

