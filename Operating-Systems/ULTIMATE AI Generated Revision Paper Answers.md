### **1. Talking to the Hardware & Booting**

**(a) Boot sequence:** The sequence begins with a physical hardware button press. The CPU then jumps to a pre-defined memory location where the initial boot code has control. This boot code initializes the hardware and then hands control over to the OS, allowing it to begin normal execution. 
**(b) Timers from interrupts:** The system clock is driven by hardware interrupts. By using periodic or auto-reload counting mechanisms, the operating system counts these regular hardware interrupt signals to build software timers. 
**(c) System calls:** User applications utilize system calls as a controlled, standardized interface to interact securely with the OS kernel.
### **2. Processes & Threads**

**(a) Process and states:** A process is a program in execution. Its five distinct lifecycle states are: running, sleeping, waiting, stopped, and zombie. 
**(b) Process management syscalls:**

- `fork()`: Creates a new child process.
- `exec()`: Replaces the current process's memory space with a new program.
- `wait()`: Pauses the parent process until the child process completes.
- `exit()`: Terminates the process. 
- **(c) Process vs Thread:** An OS process is the fundamental unit of scheduling, ownership, and administration. A thread is the actual entity of execution within that process. A single process can have many threads. **(d) Mechanism vs Policy:** A context switch is the _mechanism_ used to swap one executing thread with another by saving and restoring state (PC, SP, registers). The scheduling algorithm is the _policy_ that dictates which thread runs and when.
### **3. Process Scheduling**

**(a) CPU vs I/O bursts:** Processes experience bursts of CPU computation and bursts of I/O waiting. CPU bursts are generally shorter than I/O bursts. The scheduler uses this distinction to overlap the execution of CPU-bound tasks while I/O-bound tasks are waiting. **(b) Scheduling metrics:**

- **Throughput:** Number of tasks completed per time unit.
- **Load average:** The average number of processes in the ready or waiting states over time.
- **Turnaround time:** Total time taken from process submission to completion.
- **Waiting time:** Total time a process spends waiting in the ready queue.
- **Response time:** Time from process submission until the first output is produced. **(c) Perfect information:** Scheduling policies like Shortest Job First (SJF) require perfect information about the future (exact future CPU burst times), which is often not realistic in general-purpose computing. **(d) Priorities and Niceness:** Processes can be scheduled at different priorities. A user can apply user-level "niceness" to modify a process's priority, effectively allowing it to yield CPU time to other tasks.
### **4. Memory Management**

**(a) Bare-metal vs VM:** Bare-metal memory access is simple but inflexible. Virtual Memory (VM) creates the illusion of a large, flat main memory, providing benefits like process isolation, code relocation, and hardware abstraction. 
**(b) MMU:** The Memory Management Unit (MMU) provides the necessary hardware support to automatically translate logical (virtual) addresses into physical addresses during a page-table walk. 
**(c) Working vs Resident Set:** The working set is how many pages a process actually needs to function efficiently, while the resident set is how many pages are currently loaded into physical memory. This efficiency is empowered by temporal locality. 
**(d) NRU vs LRU:** NRU (Not Recently Used) and LRU (Least Recently Used) are page eviction policies used when physical memory is full.
### **5. Concurrency and Parallelism**

**(a) Data vs Task Parallelism:** Data parallelism involves executing the same operation simultaneously across multiple cores on different chunks of data. Task parallelism involves executing entirely different tasks simultaneously across multiple cores. 
**(b) Deadlock:** Poor use of locks (e.g., threads holding locks while waiting for others) can lead to deadlock. The four necessary conditions are: mutual exclusion, hold and wait, no-pre-emption, and circular wait. 
**(c) Amdahl's Law:** This law dictates that adding more threads is not always the solution to performance issues, as the speedup of a parallelized program is strictly limited by the portion of the program that must be executed sequentially. 
**(d) Alternative approaches:**

- **OpenMP:** Uses localized thread pools to process main thread work concurrently.
- **MPI:** Uses messages to pass data between isolated nodes, operating with no shared state.
- **OpenCL:** Involves host code invoking dedicated "kernel" code for execution on dedicated hardware accelerators.
### **6. I/O Management**

**(a) Linux device types:** The primary types are character devices (stream of bytes), block devices (fixed-size blocks for storage), and net devices (networking). 
**(b) Device drivers:** Device drivers present a standard, uniform interface to the OS, abstracting the complex and varying hardware of the actual I/O devices. 
**(c) I/O commands:**

- **Blocking:** The process pauses until the I/O operation finishes.
- **Non-blocking:** The command returns immediately, requiring the process to poll the device status.
- **Asynchronous:** The process initiates the I/O and continues working; the OS notifies the process when the data is ready.
### **7. Filesystems**

**(a) Filesystem basics:** A filesystem is the method of organizing, storing, and accessing files on a storage medium. Its primary requirements are persistence, organization, metadata, and protection. **(b) Directories:** A directory is also a file, specifically containing a collection of nodes of information about other files. While tree structures are hierarchical, general-graph structures allow complex linking. General-graph directories require garbage collection / cycle detection to prevent infinite loops during traversal. **(c) iNodes and Indirection:** Linux iNodes use hierarchical indirection (single, double, and triple indirect blocks) to map files to physical blocks. Like VM, this hierarchical indirection supports vastly larger file sizes than direct block mapping alone.
### **8. Networks**

**(a) "Everything is not a file":** While Linux treats almost everything as a file, network sockets behave differently, relying on specialized socket system calls (stream vs datagram) to handle complex network protocols. **(b) Network vs Host order:** Data must be converted between network and host order to handle the differing "endianness" (byte order) of various CPU architectures communicating across the network. **(c) eBPF architecture:** eBPF allows the execution of sandboxed code within the kernel. It uses an eBPF Verifier to ensure the code is safe to run and an eBPF JIT Compiler to translate it into efficient machine code.
### **9. System Security**

**(a) Principle of Least Privilege:** This principle limits system permissions either statically or dynamically. By compartmentalizing system and data access, it ensures that if an actor is compromised, the potential damage is heavily restricted. **(b) Protection Rings:** Protection rings act as simple levels of protection (e.g., Ring 0 to Ring N-1). They break the computer into hardware and software objects, defining exactly which actions processes in certain rings can perform on those objects. **(c) Buffer Overflow:** In C code, if a program reads or writes more data than a buffer is designed to hold without bounds checking, the excess data overflows into adjacent memory. This can overwrite variables or replace the return address, resulting in a program crash or the execution of injected arbitrary code.