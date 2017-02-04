# Adding Qt5

This build consists of a 'Hello World' program, written in C++14, with Boost and Qt5. 

This build uses the code of
[https://github.com/richelbilderbeek/travis_qmake_gcc_cpp14_boost_qt5](https://github.com/richelbilderbeek/travis_qmake_gcc_cpp14_boost_qt5).

## What is Qt5?

![The Qt logo](Qt_logo_2015.png)

Qt5 is a cross-platform GUI library.

## Cross compiling

The cross compiling script looks the same as always:

```

```

It has the following elements:

 * `#!/bin/bash`: the shebang, indicating this is a bash script
 * `cd travis_qmake_gcc_cpp14`: go into the folder `travis_qmake_gcc_cpp14`
 * `# Remove all warnings`: a comment. All comments start with a pound (`#`)
 * `sed -i 's/^QMAKE_CXXFLAGS += -W/# QMAKE_CXXFLAGS += -W/' travis_qmake_gcc_cpp14.pro`: comment out all lines
   that start with `QMAKE_CXXFLAGS += -W`. This to remove all the warnings
 * `sed -i 's/^QMAKE_CXX /# QMAKE_CXX /' travis_qmake_gcc_cpp14.pro`: comment out all lines
   that start with `QMAKE_CXX`
 * `i686-w64-mingw32.static-qmake-qt5`: create a makefile for the only `.pro` file in that folder using `Qt5`'s `qmake`
 * `make`: build the executable
 * `cd ..`: go up one folder

```
if [ ! -f ./travis_qmake_gcc_cpp14/release/travis_qmake_gcc_cpp14.exe ]
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


