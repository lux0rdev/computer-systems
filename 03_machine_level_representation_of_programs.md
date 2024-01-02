# 03: Machine-Level Representation of Programs

## 3.0 Introduction (Fundamental Concepts)

Computers execute **machine code**, _sequences of bytes encoding the low-level operations_ that manipulate data, manage memory, read and write data on storage devices, and communicate over networks. 

A **compiler** generates machine code through a series of stages, based on the rules of the programming language, the instruction set of the target machine, and the conventions followed by the operating system. 

The **gcc** C compiler generates its output in the form of **assembly code**, _a textual representation of the machine code_ giving the individual instructions in the program. 

Gcc then invokes both an **assembler** and a **linker** _to generate the executable machine code from the assembly code_. In this chapter, we will take a close look at machine code and its human-readable representation as assembly code.

Even though compilers do most of the work in generating assembly code, being able to read and understand it is an important skill for serious programmers. By invoking the compiler with appropriate command-line parameters, the compiler will generate a file showing its output in assembly-code form. By reading this code, we can understand the optimization capabilities of the compiler and analyze the underlying inefficiencies in the code.

This is important to:

- programmers seeking to maximize the performance of a critical section of code
- when the layer of abstraction provided by a high-level language hides information about the run-time behavior of a program that we need to understand
- understand how program data are shared or kept private by the different threads and precisely how and where shared data are accessed
- understand how vulnerabilities arise and how to guard against them (this requires a knowledge of the machine-level representation of programs.)

>IA32, the 32-bit predecessor to x86-64, was introduced by Intel in 1985. It served as the machine language of choice for several decades. Most x86 microprocessors sold today, and most operating systems installed on these machines, are designed to run x86-64. However, they can also execute IA32 programs in a backward compatibility mode. As a result, many application programs are still based on IA32. In addition, many existing systems cannot execute x86-64, due to limitations of their hardware or system software. IA32 continues to be an important machine language. You will find that having a background in x86-64 will enable you to learn the IA32 machine language quite readily.
>
>The computer industry has recently made the transition from 32-bit to 64- bit machines. A 32-bit machine can only make use of around 4 gigabytes (232 bytes) of random access memory, With memory prices dropping at dramatic rates, and our computational demands and data sizes increasing, it has become both economically feasible and technically desirable to go beyond this limitation. Current 64-bit machines can use up to 256 terabytes (248 bytes) of memory, and could readily be extended to use up to 16 exabytes (264 bytes).

## 3.1 A Historical Perspective

List of the most important Intel processors and some of their key features, especially those affecting machine-level programming. We use the number of transistors required to implement the processors as an indication of how they have evolved in complexity. In this table, “K” denotes 1,000 (10<sup>3</sup>), “M” denotes 1,000,000 (10<sup>6</sup>), and “G” denotes 1,000,000,000 (10<sup>9</sup>).

