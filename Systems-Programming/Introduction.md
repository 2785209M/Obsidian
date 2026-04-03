Systems programming is the process of writing computer **Systems Software**. Other software is called **Application Software**.

System Software usually has constraints such as fast execution time, low memory consumption, low energy usage, etc.

Systems programming allows for more fine grain control over the execution of programs

The main goal of this course is to learn principles and techniques of computer systems, and learning C/C++ is a byproduct.
We will learn:
- Memory and Computation as fundamental resources of computing
- Representation of Data Structures in memory and the role of data types
- Develop reasoning about concurrent systems
- Develop reasoning about the environmental impact of systems programming

C++ is C with Abstractions. It was developed by Bjarne Stroustrup.
Rust and Swift are rapidly gaining popularity.


# Programming Pradigms
**Imperative programming** 
- Computations that are sequences of statements that change a program's state
```
x = 41
x = x+1
```
**Structured Programming**
- Programs organized with subroutines and structured control flow structs
```
sum = 0
for x in array:
	sum += x
```

**Object-Oriented Programming**
- Organizes programs into objects that contain data and encapsulate behavior
```
animal = dog()
animal.makeNoise()
```

**Functional Programming**
- Mathematical functions and expressions avoiding explicit change of state
```
fib = lambda x, x_1=1, x_2=2: x_2 if x == 0 else fib(x-1, x_1 + x_2, x_1)
```

**Systems Programming Languages are generally faster and more energy efficient than other languages**

# Compiling a C program
To run a C program it must first be compiled into machine executable code
**Steps in compilation:**
1. The preprocessor expands macros
2. The source code is parsed and turned into an intermediate representation, machine-specific assembly code is generated, and finally machine code is generated in an object file
3. The linker combines multiple object files into an executable

**To compile a C program using clang:**
```
clang source.c -o program
```

# Linking
The linker combines one or more object files into a single executable
It checks that all functions called in the program have machine code available
If the machine code for a function cannot be found the linker complains