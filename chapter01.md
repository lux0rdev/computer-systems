# 01: A Tour of Computer Systems

A computer system consists of hardware and systems software that work together to run application programs. Specific implementations of system change over time, but the underlying concepts do not. All computer systems have similar hardware and software components that perform similar functions.

We will use a simple C _hello world_ program (the model program, or _the program_ from now on) for didactic purposes:

```c
#include <stdio.h>

int main()
{
  printf("hello, world\n");
  return 0;
}
```

## 1.1 Information Is Bits + Context

The program begins life as a _source program_ or _source file_. The source program is a sequence of bits (each with a value of 0 or 1) organized in 8-bit chunks called _bytes_. Each byte represents some text character in the program.

The `hello.c` program is stored in a file as a sequence of bytes. Each byte has an integer value that corresponds to some character. Files such as `hello.c` that consist exclusively of ASCII characters are known as _text files_. All other files are known as `binary files`.

All information in a system-including disk files, programs stored in memory, user data stored in memory and data transferred across a network- is represented as a bunch of bits. The only think that distinguishes different data objects is the context in which we view them.

We need to understand machine representations of numbers; they are not the same as real integers and real numbers. They are finite approximations that can behave in unexpected ways.

## 1.2 Programs Are Translated by Other Programs into Different Forms

The program begins life as a high-level C program because it can be read and understood by human beings; the individual C statements must be translated by other programs into a sequence of low-level machine_language instructions. These instructions are then packaged in a form called an executable object program and stored as a binary disk file. Object programs are also referred as executable object files.

On a Unix system, the translation from source file to object file is performed by a compiler driver:

![1.3](./images/1.png)

The translation is performed in the sequence of four phases:

- Preprocessing: the cpp preprocessor modifies the original C program according to directives that begin with `#`. In our case, this means the reading of system header files and its insertion direction into the program text.
- Compilation: The compiler cc1 translates the textile into assembly language. Assembly language is useful because it provides a common output language for different compilers for different high level languages.
- Assembly: the assembler (as) translates the file into machine-language instructions, packages them in a form known as a relocatable object program, and stores in another file.
- Linking: In this phase, functions that reside in separate pre-compiled object files, like `printf`, part of the standard C library, needs to be merged with the file. The resulting file is an executable object file ready to be loaded into memory and executed by the system.

## 1.3 It Pays to Understand How Compilation Systems Work

There are three main reasons why programmers need to understand how compilation systems work:

- Optimizing program performance: in order to make good coding decisions in our C programs, we do need a basic understanding of machine-level code and how the compiler translates different C statements into machine code.

> For example, is a `switch` statement always more efﬁcient than a sequence of if-else statements? How much overhead is incurred by a function call? Is a while loop more efﬁcient than a for loop? Are pointer references more efﬁcient than array indexes? Why does our loop run so much faster if we sum into a local variable instead of an argument that is passed by reference? How can a function run faster when we simply rearrange the parentheses in an arithmetic expression?

- Understanding link-time errors: some of the most perplexing programming errors are related to the operation of the linker, especially when you are trying to build large software systems. 

> what does it mean when the linker reports that it cannot resolve a reference? What is the difference between a static variable and a global variable? What happens if you deﬁne two global variables in different C ﬁles with the same name?

- Avoiding security holes:

> buffer overﬂow vulnerabilities have accounted for many of the security holes in network and Internet servers. These vulnerabilities exist because too few programmers understand the need
to carefully restrict the quantity and forms of data they accept from untrusted sources. A ﬁrst step in learning secure programming is to understand the consequences of the way data and control information are stored on the program stack. 

## 1.4 Processors Read and Interpret Instructions Stored in Memory

To run the executable ﬁle on a Unix system, we type its name to an application program known as a _shell_:

```text
linux> ./hello
hello, world
linux>
```

The shell is a command-line interpreter that prints a prompt, waits for you to type a command line, and then performs the command. If the ﬁrst word of the command line does not correspond to a built-in shell command, then the shell assumes that it is the name of an executable ﬁle that it should load and run.

## 1.5 Caches Matter



## 1.6 Storage Devices Form a Hierarchy



## 1.7 The Operating System Manages the Hardware



## 1.8 Systems Communicate with Other Systems Using Networks



## 1.9 Important Themes



## 1.10 Summary



