# The main() function
Every c program has one main() function which serves as an entry point to the program

The main function is very special. if the close bracket } is reached then the function automatically returns 0. A return value of 0 indicates successful execution to the environment executing the program. A non-negative return value indicates unsuccessful execution.

There are two valid versions of the main function:
```
int main(){...}
int main(int argc, char*argv[]){}
```
The second version allows for the processing of command line arguments.

# The printf() function
The printf() function is part of the C standard library, defined in stdio.h, and allows the formatted printing of values.

The format string can contain conversion specifiers such as %d(int), %f(float), %c(char), etc. for the formatting of non-string values. Full list of special characters: https://en.cppreference.com/w/c/io/fprintf

# Variables
Unlike in Python, C requires you to define datatypes when initializing variables: int x = 4.
This is because C is **Statically Typed**. 
In C, a char is actually a 1 byte representation of an integer.
Also in C, every integer can be represented as a boolean. 0 is false and all values above 0 are true.
By using the <stbool.h> header it is possible to declare "true" and "false" as macros with the values 1 and 0.

# Data Types / Bit Patterns
**Bit patterns** alone have no inherent meaning. They must be given a meaning. We, as programmers, define the meaning through data types. We associate specific **bit patterns** with specific **data types**. The data offers meaning to the bits, telling the program what the bits in memory represent.

Every variable in C is stored at a fixed location in memory. This memory location does not change over the variable's lifetime.

In C, the data type of a variable determines its location in memory.
```
int x = 42;   // represented as: 0000 0000 0000 0000 0000 0000 0010 1010
float f = 42; // represented as: 0100 0010 0010 1000 0000 0000 0000 0000
```
By choosing a specific data type we are in control of how much memory we use.

| Datatype  | Storage (bytes) |
| --------- | --------------- |
| Char      | 1               |
| short     | 2               |
| int       | 4               |
| float     | 4               |
| long      | 4-8             |
| long long | 8               |
| double    | 8               |


# Lexical Scoping
In C each pair of curly braces {} is called a **Block**, and introduces **Lexical Scope**.
- Variables inhabiting the same lexical scope must be unique
- If multiple variables with the same name exist in nested scope, the **Innermost** scope is used
- This is called **Shadowing**


# Scoping and Preprocessor macros
Preprocessor macros are dangerous as they don't respect lexical scoping

consider the following macro:
```
#define ADD_A(x) x + a
```
This macro cannot be defined in its own scope because a and x don't exist in it

But, if you use the macro within a function where x and a do exist, it will work.

This is called dynamic scoping and should be avoided.


# Variable lifetime
**automatic:**
- If a variable is declared in a block {} its lifetime ends at the end of the block. (stack)
**Static:**
- Variables defined outside of blocks (in white space) or with the **static** keyword will live until the program terminates. (stack)
**allocated:**
- allocated variables are variables for which we have specifically allocated memory using malloc() and will live until manually freed using free(). (heap)

# Stack-based memory management
Automatically-managed variables are also considered **stack objects**
When their lifetime ends their space in memory can be reused by another variable

The stack is an area of memory reserved for the program. It is last-in first-out and automatically managed.
- Every time a block is entered { put aside a location in memory for every variable declared in that block
- Every time a block is exited } free the locations in memory for every variable declared in that block

# Arrays
Arrays consist of multiple elements of the same type. 
Elements in an array are stored next to each other in memory.
Arrays stored on the stack must have a fixed size.
Arrays with a size that can change are stored on the heap.

# Structs
A struct consists of a sequence of "members" of (potentially) different types.
Members in a struct can be accessed using . notation.
```
struct point{
int x;
int y;
};
int main(){
struct point p = {1,2};
printf("%d", p.x)
}
```
Members are stored next to each other in memory
The type of a struct is written "struct name", but we often use this trick to shorten it:
```
typedef struct{
int x;
int y;
} point;
int main() {point p = {1, 2};}
```

# String
Strings in C are represented as an array of characters.
We use double quotes " " to write an ASCII string literal and single quotes ' ' to write a  character literal.
Strings in C are terminated by the special character '\0' which is added automatically for string literals.
To print a string using printf we use %s


# Functions
Functions in C consist of a **return type, function name, parameter list, and function body**.
```
return_type function_name(parameter_list){function_body}
```

## Declaration vs Definition
**Function Definition** fully specifies the behaviour of a function
**Function Declaration** only specifies how a function can be used - the interface
```
int max(int lhs, int rhs)
```

Why use Declarations?
They separate the interface from the implementation. Useful for writing modular software.
A function declaration essentially tells the compiler that a function definition exists "somewhere else", and the linker has to go and find the real function's memory address, at which point the placeholder declaration will be replaced with the real function. E.g. classes and sub-classes. Used a lot.

## Header files
Header files hold common declarations such as function declarations, type definitions, and macros. The .h (header) files contain declarations for functions, whilst the .c files (program) hold the definitions for functions.


## Call-by-value
When you pass a variable into a function a copy of it is made and stored at a different location in memory. Specifically it is copied into the function's parameters and the copy is stored at the parameter's location in memory. This means the original variable will remain unchanged.

Functions can accept structs and arrays as parameters.
Structs behave like basic types and are passed-by-value.
Arrays are treated differently. Instead of copying the array, only the address of the first element is passed. Because of this, changes to the array are now visible outside the function.

