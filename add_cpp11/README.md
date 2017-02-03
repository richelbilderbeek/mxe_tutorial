# Adding C++11

This build consists of a 'Hello World' program, written in C++11. 

This build uses the code of
[https://github.com/richelbilderbeek/travis_qmake_gcc_cpp11](https://github.com/richelbilderbeek/travis_qmake_gcc_cpp11).

## What is C++11?

C++11 is a C++ standard finalized in 2011.

## Cross compiling

The cross compiling script looks like this:

```
#!/bin/bash
cd travis_qmake_gcc_cpp11

# Remove all warnings
sed -i 's/^QMAKE_CXXFLAGS = -W/# QMAKE_CXXFLAGS = -W/' travis_qmake_gcc_cpp11.pro
# Remove use of compiler and linker 
sed -i 's/^QMAKE_CXX /# QMAKE_CXX /' travis_qmake_gcc_cpp11.pro
sed -i 's/^QMAKE_LINK/# QMAKE_LINK/' travis_qmake_gcc_cpp11.pro
sed -i 's/^QMAKE_CC/# QMAKE_CC/' travis_qmake_gcc_cpp11.pro

i686-w64-mingw32.static-qmake-qt5

make

cd ..

if [ ! -f ./travis_qmake_gcc_cpp11/release/travis_qmake_gcc_cpp11.exe ]
then
  echo "Error: Windows executable has not been created"
  exit 1
fi
```

It has the following elements:

 * `#!/bin/bash`: the shebang, indicating this is a bash script
 * `cd travis_qmake_gcc_cpp11`: go into the folder `travis_qmake_gcc_cpp11`
 * `# Remove all warnings`: a comment. All comments start with a pound (`#`)
 * `sed -i 's/^QMAKE_CXXFLAGS += -W/# QMAKE_CXXFLAGS += -W/' travis_qmake_gcc_cpp11.pro`: comment out all lines
   that start with `QMAKE_CXXFLAGS += -W`. This to remove all the warnings
 * `sed -i 's/^QMAKE_CXX /# QMAKE_CXX /' travis_qmake_gcc_cpp11.pro`: comment out all lines
   that start with `QMAKE_CXX`
 * `i686-w64-mingw32.static-qmake-qt5`: create a makefile for the only `.pro` file in that folder using `Qt5`'s `qmake`
 * `make`: build the executable
 * `cd ..`: go up one folder

```
if [ ! -f ./travis_qmake_gcc_cpp11/release/travis_qmake_gcc_cpp11.exe ]
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


