### **1. Processes and Scheduling (12 Marks)**

**(a) Define a process and list the five standard lifecycle states it can transition through. [3]**

- **1.5 marks:** A process is defined as a program currently in execution. It serves as the operating system's primary unit of scheduling, ownership, and administration.
- **1.5 marks:** The five standard lifecycle states are running, sleeping, waiting, stopped, and zombie.

**(b) Differentiate between an OS process and a thread. Explain how threads provide a programming abstraction for execution. [3]**

- **1.5 marks:** While a process acts as the container for resources and administration, a thread is the actual entity of execution within that process. A single process can contain many threads that share its memory and resources.
- **1.5 marks:** Threads provide a programming abstraction for concurrent or parallel execution, allowing developers to break a program down into multiple independent execution streams that can run simultaneously.

**(c) Detail the exact steps of a context switch, specifically mentioning the role of the Program Counter (PC), Stack Pointer (SP), registers, and the Process Control Block (PCB) or Thread Control Block (TCB). [3]**

- **1 mark:** The OS interrupts the currently running thread and saves its current state (including the PC, SP, and CPU registers) into its Thread Control Block (TCB), which resides inside the Process Control Block (PCB).
- **1 mark:** The OS scheduler then selects the next thread to run.
- **1 mark:** The OS restores the state of the newly selected thread by loading its PC, SP, and registers from its respective TCB/PCB, allowing it to resume execution exactly where it left off.

**(d) Contrast a First-Come, First-Served (FCFS) scheduling policy with a Round Robin (RR) scheduling policy. Why is it critical to set the RR scheduling quantum to an appropriate value? [3]**

- **1 mark:** FCFS is a non-preemptive policy where tasks are executed to completion in the exact order they arrive, whereas RR is a preemptive policy that cycles through the ready queue, giving each task a specific time slice (quantum) on the CPU.
- **1 mark:** If the RR quantum is set too small, the system will spend an excessive amount of time performing context switches (overhead) rather than executing actual task work.
- **1 mark:** If the RR quantum is set too large, the system's responsiveness decreases, and the scheduling behavior devolves into an FCFS system.
### **2. Memory Management (12 Marks)**

**(a) Explain the fundamental trade-off between memory size and access times, and briefly describe how the OS tries to amortize this trade-off. [2]**

- **1 mark:** Memory systems face a physical trade-off: larger memory capacities (like HDDs/SSDs) suffer from slower access times, while the fastest memory (like CPU cache) is highly constrained in size.
- **1 mark:** The OS amortizes this trade-off using Virtual Memory (VM), creating the illusion of a large, flat main memory by moving frequently accessed data to faster storage (RAM) and leaving less critical data on slower backing storage.

**(b) Virtual memory eliminates external fragmentation but introduces internal fragmentation. Explain why this happens, making reference to page and frame sizes. [3]**

- **1.5 marks:** Virtual memory divides logical memory into fixed-size "pages" and physical memory into fixed-size "frames" of the exact same size. Because any page can fit into any available frame, contiguous physical memory is no longer required, entirely eliminating external fragmentation.
- **1.5 marks:** However, if a process requests memory that is not an exact multiple of the page size, the final allocated page will not be completely filled, resulting in wasted space inside the page itself, known as internal fragmentation.

**(c) Describe the page-table walk process for logical-to-physical address translation. Include the role of the Translation Look-aside Buffer (TLB) and how it minimizes effective access time. [4]**

- **2 marks:** The page-table walk is an automatic hardware process where the Memory Management Unit (MMU) uses the logical address provided by the CPU to index into the in-memory page table, looking up the corresponding physical frame number.
- **2 marks:** The TLB acts as a high-speed hardware cache specifically for these page table entries. By checking the TLB in parallel with the page table lookup, the OS can bypass the slow memory access required for a full page-table walk if the translation is already cached (a TLB hit), vastly minimizing effective access time.

**(d) Explain the phenomenon of _thrashing_. What is the worst-case memory scenario resulting from this, and how does the Out-of-Memory (OOM) Killer intervene? [3]**

- **1 mark:** Thrashing occurs when the system is starved of physical memory and spends more time actively swapping pages in and out of backing storage than it does performing meaningful computation.
- **1 mark:** The worst-case scenario is a complete exhaustion of physical memory where the OS can no longer allocate pages for essential tasks.
- **1 mark:** To prevent a total system crash, the kernel invokes the OOM Killer, a process that forcibly terminates processes to free up RAM.
### **3. Concurrency and Parallelism (11 Marks)**

**(a) Concurrency and parallelism are often confused. Define both terms and explain the difference between _data parallelism_ and _task parallelism_. [3]**

- **1 mark:** Concurrency is a property of the workload where more than one task is making progress at the same time, whereas parallelism is a property of the system where multiple CPU cores allow actual simultaneous execution.
- **1 mark:** Data parallelism involves splitting a large dataset into chunks and having multiple threads perform the exact same operation on different chunks simultaneously.
- **1 mark:** Task parallelism involves multiple threads executing entirely different tasks or operations at the same time.

**(b) Define a _critical region_. Why is serialized access necessary when multiple threads attempt to interact with a shared resource? [2]**

- **1 mark:** A critical region is a specific area of code that accesses a shared common resource or variable.
- **1 mark:** Serialized access (using locks) is necessary to protect critical regions from race conditions, ensuring that only one thread can manipulate the shared state at a time, preventing data corruption.

**(c) Identify the four necessary conditions for a deadlock to occur in a concurrent system. [4]**

- _Award 1 mark for each condition:_
- Mutual exclusion.
- Hold and wait.
- No pre-emption.
- Circular wait.

**(d) Briefly compare user-space synchronization mechanisms with kernel-space mechanisms. [2]**

- **1 mark:** User-space synchronization (like mutexes and semaphores) provides abstractions for developers to lock resources within their applications without immediately requiring a system call.
- **1 mark:** Kernel-space mechanisms (like atomics, memory barriers, spin locks, and futexes) operate at a lower level, managing hardware synchronization and OS-level locking logic.

### **4. Filesystems and I/O (13 Marks)**

**(a) The Linux operating system heavily relies on the "Everything is a file" abstraction. Describe this concept and provide two benefits it offers to the OS and developers. [3]**

- **1 mark:** "Everything is a file" means that the OS treats documents, directories, hardware devices, and even network sockets as file streams that can be interacted with using standard file system calls.
- **2 marks:** This provides a standard abstraction layer, ensuring system consistency and vastly simplifying administration and development.

**(b) Compare and contrast the following methods of mapping files to physical blocks on a storage medium: _contiguous_, _linked_, and _indexed_. [6]**

- **2 marks (Contiguous):** Allocates a single, continuous sequence of physical blocks for a file. It offers excellent read performance but suffers from severe external fragmentation and difficulty in expanding files.
- **2 marks (Linked):** Each block contains a pointer to the next block in the file, scattering data across the disk. This solves fragmentation and allows easy file growth, but it makes direct (random) access very slow.
- **2 marks (Indexed):** Uses a dedicated index block to store an array of pointers mapping to the scattered data blocks. This supports fast direct access and prevents external fragmentation, but requires overhead to store the index block itself.

**(c) In the context of device management, contrast the user-space I/O commands of _blocking I/O_, _non-blocking I/O_, and _Asynchronous I/O_. [4]**

- **1.5 marks:** Blocking I/O forces the calling process to sleep (wait) until the entire I/O operation completes, whereas non-blocking I/O returns immediately, often requiring the program to repeatedly poll to see if the data is ready.
- **2.5 marks:** Asynchronous I/O allows the process to initiate an I/O request and immediately continue doing other work. The OS then actively notifies the process (e.g., via a callback or signal) once the data transfer is fully complete.

### **5. Networks and System Security (12 Marks)**

**(a) In a distributed system, how does a Local Area Network (LAN) differ from a Wide Area Network (WAN)? [2]**

- **1 mark:** A LAN connects loosely coupled nodes within a highly localized area.
- **1 mark:** A WAN spans much larger geographical distances, connecting nodes across cities or countries.

**(b) In Linux network programming, data must often be converted between "network order" and "host order." Briefly explain why this is necessary and how socket buffers help optimize networking operations in the kernel. [3]**

- **1.5 marks:** Data conversion is necessary because different system architectures represent multi-byte data structures differently (endianness); "network order" provides a standardized format for safe transmission across diverse nodes.
- **1.5 marks:** Socket buffers are used to significantly reduce `memcpy()` operations in the kernel, minimizing the overhead of copying data between the various networking layers.

**(c) Discuss the concept of _Protection Rings_ in system security. How do they establish domains of protection for hardware and software objects? [3]**

- **1.5 marks:** Protection rings are simple hierarchical levels of privilege within a system (e.g., Ring 0 to Ring N-1), where lower numbers dictate higher trust and core kernel access.
- **1.5 marks:** They establish domains of protection by defining exactly which actions processes can perform on specific hardware and software objects based on their current operating ring, preventing user-space apps from executing critical hardware instructions.

**(d) Explain the mechanism of a buffer overflow attack using C strings. What might happen as a consequence of a successful overflow, and how can an operating system mitigate it? [4]**

- **1.5 marks:** A buffer overflow occurs when a program using unsafe string handling (common in C) reads or writes more data than a target buffer was allocated to hold.
- **1.5 marks:** This spills over into adjacent memory, overwriting other variables or replacing the return address, which can lead to a program crash or allow the attacker to execute arbitrary injected code.
- **1 mark:** An OS can mitigate this by enforcing bounds checking and adding padding to ensure data does not exceed its intended limits.