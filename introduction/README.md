# Introduction

This is an MXE tutorial, version 0.1.

##  License

This tutorial is licensed under Creative Commons license 4.0. 

![CC-BY-SA](CC-BY-SA_icon.png)

All C++ code is licensed under GPL 3.0.

![GPL v3.0](gplv3.png)

## Crosscompiling to Windows

### The benefits of developing code under GNU/Linux

Developing C++ code under GNU/Linux has advantages: one can easily install and use FOSS software and libraries from the command-line. When using an online git repository like GitHub, it is easy to add continuous integration, for example Travis CI.

### The customer needs

Customers generally deploy to the Microsoft Windows operating system, as it has the largest market share. This may force a developer into buying a Microsoft computer just for deployment.

### The solution

MXE, shorthand for 'M cross Environment', is a cross-compiling environment that allows compiling C++ code to a statically linked Windows executable. It can be used from the command line.

##  Tutorial style

This tutorial is aimed at the beginner.

### Introduction of new terms and tools

All terms and tools are introduced shortly once, by a 'What is' paragraph. This allows a beginner to have a general idea about what the term/tool is, without going in-depth. Also, this allows for those more knowledgeable to skim the paragraph.

### Repetitiveness

To allow skimming, most chapters follow the same structure. Sometimes the exact same wording is used. This is counteracted by referring to earlier chapters.

### Ties to the Travis C++ tutorial

This tutorial follows the same structure as the Travis C++ Tutorial, which is available online at https://github.com/richelbilderbeek/travis_cpp_tutorial.

##  This tutorial

This tutorial is available online at https://github.com/richelbilderbeek/mxe_tutorial. 

I cannot have it checked by Travis, as building MXE would go beyon the one hour alotted for a free user.

##  Acknowledgements

These people contributed to this tutorial:

 * [None yet]

##  Collaboration

I welcome collaboration for this tutorial, especially in getting the scripts as clean as possible. If you want to help scraping off some lines, I will be happy to make you a collaborator of some GitHubs. 

##  Feedback

This tutorial is not intended to be perfect yet. For that, I need help and feedback from the community. All referenced feedback is welcome, as well as any constructive feedback. 