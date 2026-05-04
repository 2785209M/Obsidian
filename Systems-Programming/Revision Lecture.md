# Part 1: Systems Programming
## TLDR
We should be comfortable reading and writing C code that uses:
- Pointers and pointer arithmetic
- Structs and arrays
- Dynamic memory allocation
- Dynamic Data Structures
## In this course, we have learned about:
- Data types giving meaning to bits in memory
- Structs allowing us to implement custom data structures
- Every variable being stored at a fixed memory location
- Memory on the stack  is managed automatically
- Memory on the heap is managed manually
- We use ownership to organize resource and memory management
- In C++ ownership is implemented by tying resource management to the lifetime of a stack variable
- There is a set of smart pointers and containers for easy and non-leaking memory management
## Basics of C
You should be able to:
- Select appropriate datatypes
- Distinguish between variable lifetimes and predict how variables behave when their scope ends
- Understand the implications of passing function arguments by value in C
## Memory and Pointers
You should be able to:
- Work with Pointers in C
- Debug common pointer-related issues
- Allocate and free memory dynamically
- Avoid common pitfalls such as returning pointers to local stack variables
- Design and build dynamic data structures  e.g. linked lists, binary search trees
- be able to utilize pointer arithmetic and proper pointer manipulation to traverse/modify data structures
## Debugging and Dev tools
You should be able to:
- Distinguish between syntactic and semantic errors
- Pinpoints common issues of segmentation faults and explain how each leads to invalid memory access
- Examine cases of undefined behaviour and understand how they can lead to unpredictable behaviour
## Memory Management and Ownership
You should be able to:
- Enforce memory ownership via RAII and design C++ Structures/classes that automatically manage the lifetime of resources
- Utilize C++ smart pointers to ensure that dynamically allocated memory is correctly and safely managed
## Energy efficient and sustainable Computing
You should be able to:
- Distinguish between execution time, power, energy, and carbon footprint in computing
- Recognize coding practices that improve energy efficiency in C/C++

# Part 2: concurrency
## TLDR
You should be comfortable reading and writing code that uses:
- Threads
- Shared Memory and critical regions
- Mutexes/locks
- Condition variables
- Semaphores, barriers, and atomics
- Asynchronous Tasks
## Concurrency Concepts
You should be able to:
- Explain what concurrency is and distinguish it from parallelism
- Describe the difference between processes and threads
- Implement simple multi threaded programs using POSIX threads
- Recognize race conditions
- Apply locks/mutexes
- Explain how a deadlock can arise
- Use condition variables to coordinate threads to avoid busy waiting
## C++ threading
You should be able to:
- Create and manage C++ threads safely
- Use mutexes and condition variables for C++ thread synchronization
- Distinguish between C++ threads and tasks
- Launch async tasks and synchronize/communicate using futures/promises
## Synchronization mechanisms
You should be able to:
- Understand and explain why some condition variable solutions may still be incorrect
- Describe monitors, semaphore, and barriers
- Use semaphores
- Explain how barriers coordinate phases of multi threaded execution
- Understand the role of atomics

# Nikela said
We must be comfortable with implementing linked lists and trees
We will mostly be asked to fix and write small sections of code
There are no word limits in the questions this year
She will mark your answers to the 2024 paper if you send them to her