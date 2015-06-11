# Lecture 11 Preprocessor Classes
#### Annocements
See valgrind Post on piazza. @619  
your progs for a2 should not leak memory.

## Preprocessor Recap

`$> g++ -E -P  file` just runs preprocessors  
`#include ` copy and paste header file  
`#define VAR VALUE`

 - create preprocessor variable
 - search and replace

Note that we should use the `const` keyword instead of `#define` for constants.


## Conditional Compilation
see file preprocessor_condcompile.cc  
The code requires manually changing the value for the variable OS.  
Note that `#if` in the file is called if-condtion preprocessor.

To be more convenient, we can define preprocessor variables as compiler arguments.
Use the option: `-DVAR=VALUE`, where `VAR` is variable, `VALUE` is the value you 
want to set the variable to.

`g++ -DOS=Unix condcompile_commandline.cc`  
Note that it is case sensitive.

#### Example
```c++
#define ever ;;
for (ever) { // funny
}
```

We can set variable without giving it a value 
```
#define MyVar
```
Value of `MyVar` default to an empty string.

`#ifdef VAR` is true if VAR is defined,  
`#ifndef VAR` is true if VAR is not defined.

e.g.  
`g++ -DUnix condcompile_commandline_defs.cc`  
`g++ -DWindows condcompile_commandline_defs.cc`

`-D` option can be useful for debugging.  
`g++ -DDubug file.cc`  
Moreover, we can write a function called `debug` that reads and returns 
strings to save writing and spaces. (did not expand much)


## Separate Compilation
A program can be divided into:

- interface (header, .h): contains type definitions, function headers
- implementation (.cc): actual implementation of functions

see example in `separate/example1` directory.

How to compile the files: 

1. `g++ *.cc`
2. `g++ main.cc vector.cc`  (specify the files)

**Never** compile header file (.h), only include them in implementation files. 

However, separate compilation is more powerful than that.  
Separate compilation means the ability to compile individual pieces of code.
It has the advantages that 

- reduces the compile time
- helps debugging

When we do `g++ main.cc`, it does 

 - preprocessor
 - compiler
 - linkers
 - produce executable

Use the -c option to only compile, without link.  
`-c` option creates .o file (object file), which contains

 - compiled binary
 - info of what this portion of the prog needs and what it has available

The linkers matches the availability and needs of different .o files.  
**Never** compile .h files, and **never** include .cc files

## Global Variables
We can place a global in a header file which can be included by other files.

In abc.h,
```
int global;  // this is both a declaration and a definition
```

When including the abc.h file in multiple .cc files, they will all have the
above definition of variable `global`. It causes multiple definition of 
link time. it is **NOT GOOD**.  

Solution: to only declare, not define, a global in the headers file, we use 
the keyword "extern", so we write `extern int global;` in abc.h, and write 
`int global;` in abc.cc.

Also, be careful that structures will have similar "include" issues.

