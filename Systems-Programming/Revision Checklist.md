## Part 1: Systems Programming

### General Concepts & TLDR Review

- [ ] Practice reading and writing C code using pointers, pointer arithmetic, structs, arrays, and dynamic memory allocation.
    
- [ ] Build and manipulate dynamic data structures in C.
    
- [ ] Explain how data types give meaning to bits in memory.
    
- [ ] Use structs to implement custom data structures.
    
- [x] Understand that variables are stored at fixed memory locations.
    
- [x] Differentiate between automatic memory management (stack) and manual memory management (heap).
    
- [ ] Explain the concept of ownership for organizing resource and memory management.
    
- [ ] Understand how C++ implements ownership by tying resource management to the lifetime of a stack variable.
    
- [ ] Review the use of smart pointers and containers for easy, leak-free memory management.
    

### Basics of C

- [x] Select the appropriate datatypes for various scenarios.
    
- [ ] Distinguish between variable lifetimes and predict how variables behave when their scope ends.
    
- [ ] Explain the implications of passing function arguments by value in C.
    

### Memory and Pointers

- [ ] Work proficiently with pointers in C.
    
- [ ] Debug common pointer-related issues.
    
- [ ] Allocate and free memory dynamically.
    
- [ ] Identify and avoid common pitfalls, such as returning pointers to local stack variables.
    
- [ ] Design and build dynamic data structures (e.g., linked lists, binary search trees).
    
- [ ] Utilize pointer arithmetic and proper pointer manipulation to traverse and modify data structures.
    

### Debugging and Dev Tools

- [ ] Distinguish between syntactic and semantic errors.
    
- [ ] Pinpoint common causes of segmentation faults and explain how each leads to invalid memory access.
    
- [ ] Examine cases of undefined behavior and understand how they lead to unpredictable results.
    

### Memory Management and Ownership

- [ ] Enforce memory ownership via RAII (Resource Acquisition Is Initialization).
    
- [ ] Design C++ structures/classes that automatically manage the lifetime of resources.
    
- [ ] Utilize C++ smart pointers to ensure dynamically allocated memory is correctly and safely managed.
    

### Energy Efficient and Sustainable Computing

- [ ] Distinguish between execution time, power, energy, and carbon footprint in computing.
    
- [ ] Recognize and apply coding practices that improve energy efficiency in C/C++.
    

---

## Part 2: Concurrency

### General Concepts & TLDR Review

- [ ] Practice reading and writing code that utilizes threads, shared memory, and critical regions.
    
- [ ] Review the implementation and usage of mutexes/locks and condition variables.
    
- [ ] Understand advanced synchronization tools: semaphores, barriers, and atomics.
    
- [ ] Review the implementation of asynchronous tasks.
    

### Concurrency Concepts

- [x] Explain what concurrency is and distinguish it from parallelism.
    
- [x] Describe the fundamental differences between processes and threads.
    
- [ ] Implement simple multithreaded programs using POSIX threads (`pthreads`).
    
- [ ] Recognize and resolve race conditions.
    
- [ ] Apply locks/mutexes to protect shared resources.
    
- [x] Explain the conditions under which a deadlock can arise.
    
- [ ] Use condition variables to coordinate threads and avoid busy waiting.
    

### C++ Threading

- [ ] Create and manage C++ threads safely.
    
- [ ] Use mutexes and condition variables specifically for C++ thread synchronization.
    
- [ ] Distinguish between C++ threads and tasks.
    
- [ ] Launch async tasks and synchronize/communicate using futures and promises.
    

### Synchronization Mechanisms

- [ ] Understand and explain why some condition variable solutions may still be incorrect (e.g., spurious wakeups).
    
- [ ] Describe the concepts of monitors, semaphores, and barriers.
    
- [ ] Implement synchronization using semaphores.
    
- [ ] Explain how barriers coordinate distinct phases of multithreaded execution.
    
- [ ] Understand the role and application of atomics in concurrent programming.