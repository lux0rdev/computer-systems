# 02: Representing and Manipulating Information

Modern computers store and process information represented as two-valued signals. These lowly binary digits, or bits, form the basis of the digital revolution.

Binary values work better when building machines that store and process information. Two-valued signals can readily be represented, stored, and transmitted. The electronic circuitry for storing and performing computations on two-valued signals is very simple and reliable, enabling manufacturers to integrate millions, or even billions, of such circuits on a single silicon chip.

When we group bits together and apply some interpretation that gives meaning to the different possible bit patterns, however, we can represent the elements of any finite set (numbers, letters and symbols, operations...)

We consider the three most important representations of numbers:

- Unsigned encodings are based on traditional binary notation, representing numbers greater than or equal to 0. 
- Two’s-complement encodings are the most common way to represent signed integers, that is, numbers that may be either positive or negative. 
- Floating-point encodings are a base-2 version of scienti?c notation for representing real numbers. Computers implement arithmetic operations, such as addition and multiplication, with these different representations, similar to the correspond- ing operations on integers and real numbers.

Computer representations use a limited number of bits to encode a number, and hence some operations can **overflow** when the results are too large to be represented. For example, in a 32 bit machine,

```t
200 * 300 * 400 * 500
```

yields 

```t
-884,901,888
```

**Integer** computer arithmetic satisfies many of the familiar properties of true integer arithmetic, like associative and conmutative; all these expressions yield the same result `-884,901,888`:

```t
(500 * 400) * (300 * 200)
((500 * 400) * 300) * 200
((200 * 500) * 300) * 400
400 * (200 * (300 * 500))
```

**Floating-point** arithmetic has altogether different mathematical properties. The product of a set of positive numbers will always be positive, although overflow will yield the special value +8. Floating-point arithmetic is not associative due to the finite precision of the representation.

The different mathematical properties of integer versus floating-point arithmetic stem from the difference in how they handle the finiteness of their representations—integer representations can encode a comparatively small range of values, but do so precisely, while floating-point representations can encode a wide range of values, but only approximately.

By studying the actual number representations, we can understand the ranges of values that can be represented and the properties of the different arithmetic operations. This understanding is critical to writing programs that work correctly over the full range of numeric values and that are portable across different combinations of machine, operating system, and compiler.

A number of computer security vulnerabilities have arisen due to some of the subtleties of computer arithmetic.

We will derive several ways to perform arithmetic operations by directly manipulating the bit-level representations of numbers Understanding these techniques will be important for understanding the machine-level code generated by compilers in their attempt to optimize the performance of arithmetic expression evaluation.

The C++ programming language is built upon C, using the exact same numeric representations and operations. Everything said in this chapter about C also holds for C++.

## 2.1 Information Storage

Rather than accessing individual bits in memory, most computers use blocks of 8 bits, or **bytes**, as the smallest addressable unit of memory. A machine-level program views memory as a very large array of bytes, referred to as **virtual memory**. Every byte of memory is identified by a unique number, known as its **address**, and the set of all possible addresses is known as the **virtual address space**.

This virtual address space is just a conceptual image presented to the machine-level program. The actual implementation uses a combination of dynamic random access memory (DRAM), flash memory, disk storage, special hardware, and operating system software to provide the program with what appears to be a monolithic byte array.

The compiler and run-time system partitions this memory space into more manageable units to store the different program objects, that is, program data, instructions, and control information. Various mechanisms are used to allocate and manage the storage for different parts of the program. This management is all performed within the virtual address space.

>For example, the value of a pointer in C—whether it points to an integer, a structure, or some other program object—is the virtual address of the first byte of some block of storage.
>
>The C compiler also associates type information with each pointer, so that it can generate different machine-level code to access the value stored at the location designated by the pointer depending on the type of that value. Although the C compiler maintains this type information, the actual machine-level program it generates has no information about data types. It simply treats each program object as a block of bytes and the program itself as a sequence of bytes.

>Pointers are a central feature of C. They provide the mechanism for referencing elements of data structures, including arrays. Just like a variable, a pointer has two aspects: its value and its type. The value indicates the location of some object, while its type indicates what kind of object (e.g., integer or floating-point number) is stored at that location.
> 
>Truly understanding pointers requires examining their representation and implementation at the machine level.

### 2.1.1 Hexadecimal Notation

A single byte consists of 8 bits. In binary notation, its value ranges from 00000000 to 11111111. When viewed as a decimal integer, its value ranges from 0 to 255.

Neither notation is very convenient for describing bit patterns; we write bit patterns as base-16, or hexadecimal numbers.

![Hexadecimal Notation](/images/6.png)

### 2.1.2 Data Sizes

Every computer has a **word** size, indicating the nominal size of pointer data. Since a virtual address is encoded by such a word, the most important system parameter determined by the word size is the maximum size of the virtual address space. The word size determines the maximum memory address number it can represent; that is, for a machine with a w-bit word size, the virtual addresses can range from 0 to 2<sup>w</sup> - 1, giving the program access to at most 2<sup>w</sup> bytes.

In recent years, there has been a widespread shift from machines with 32- bit word sizes to those with word sizes of 64 bits. A 32-bit word size limits the virtual address space to 4 gigabytes (written 4 GB), that is, just over 4 × 10<sup>9</sup> bytes. Scaling up to a 64-bit word size leads to a virtual address space of 16 exabytes, or around 1.84 × 10<sup>19</sup> bytes.

Most 64-bit machines can also run programs compiled for use on 32-bit ma- chines, a form of backward compatibility.

Computers and compilers support multiple data formats using different ways to encode data, such as integers and floating point, as well as different lengths.

The C language supports multiple data formats for both integer and floating- point data. Figure 2.3 shows the number of bytes typically allocated for different C data types.


![Type sizes](/images/7.png)

Programmers should strive to make their programs portable across different machines and compilers. One aspect of portability is to make the program insensitive to the exact sizes of the different data types. The C standards set lower bounds on the numeric ranges of the different data types, as will be covered later, but there are no upper bounds (except with the fixed-size types).

### 2.1.3 Addressing and Byte Ordering

For program objects that span multiple bytes, we must establish two conventions: what the address of the object will be, and how we will order the bytes in memory. In virtually all machines, a multi-byte object is stored as a contiguous sequence of bytes, with the address of the object given by the smallest (the "first") address of the bytes used.

For ordering the bytes representing an object, there are two common conventions:

- Some machines choose to store the object in memory ordered from the least significant byte to most, while other machines store them from most to least. The former convention—where the least significant byte comes first—is referred to as **little endian**. 
- The latter convention—where the most significant byte comes first—is referred to as **big endian**.

Most Intel-compatible machines operate exclusively in little-endian mode. On the other hand, most machines from IBM and Oracle (arising from their acquisition of Sun Microsystems in 2010) operate in big-endian mode. Many recent microprocessor chips are bi-endian, meaning that they can be con?gured to operate as either little- or big-endian machines. In practice, however, byte ordering becomes fixed once a particular operating system is chosen.

There is no technological reason to choose one byte ordering convention over the other, and hence the arguments degenerate into bickering about sociopolitical issues. As long as one of the conventions is selected and adhered to consistently, the choice is arbitrary. For most application programmers, the byte orderings used by their machines are totally invisible; programs compiled for either class of machine give identi- cal results. At times, however, byte ordering becomes an issue:

- when binary data are communicated over a network between different machines. A common problem is for data produced by a little-endian machine to be sent to a big-endian machine, or vice versa, leading to the bytes within the words being in reverse order for the receiving program. To avoid such problems, code written for networking applications must follow established conventions
- A second case where byte ordering becomes important is when looking at the byte sequences representing integer data. This occurs often when inspecting machine-level programs.
- when programs are written that circumvent the normal type system. In the C language, this can be done using a cast or a union to allow an object to be referenced according to a different data type from which it was created.