**VENETAS VITA** **University of Glasgow**

**OPERATING SYSTEMS (H)** **COMPSCI4011**

**THE ULTIMATE REVISION PAPER**

**RUBRIC: Answer All 9 Questions** **This examination paper is designed to cover the entire course payload.**

---

### **1. Talking to the Hardware & Booting**

(a) Detail the computer booting sequence, starting from the physical button press to the OS taking control. 
(b) Explain the mechanism of building system timers from hardware interrupts. 
(c) How do user applications utilize system calls to interact securely with the operating system?
### **2. Processes & Threads**

(a) Define an OS process and identify the five distinct lifecycle states it can experience (running, sleeping, waiting, stopped, zombie). 
(b) Review the specific roles of the `fork()`, `exec()`, `wait()`, and `exit()` system calls in process lifecycle management. 
(c) Differentiate between an OS process (as a unit of ownership) and a thread (as a unit of execution). 
(d) Contrast the _mechanism_ of a context switch with the _policy_ of a scheduling algorithm.
### **3. Process Scheduling**

(a) Differentiate between CPU bursts and I/O bursts. Why is this distinction important for a scheduler? 
(b) Define the following key scheduling metrics: throughput, load average, turnaround time, waiting time, and response time. 
(c) Explain the inherent limitations of scheduling policies (like Shortest Job First) that require perfect information about the future. 
(d) Describe how priorities and user-level "niceness" interact to affect process scheduling in Linux.
### **4. Memory Management**

(a) Compare the mechanics of bare-metal memory access with the benefits provided by Virtual Memory (VM), specifically noting process isolation and hardware abstraction. 
(b) Describe the role of the Memory Management Unit (MMU) in translating logical addresses to physical addresses. 
(c) Differentiate between the _working set_ and the _resident set_ of a process, making specific reference to the concept of temporal locality. 
(d) Compare the NRU (Not Recently Used) and LRU (Least Recently Used) page eviction policies.
### **5. Concurrency and Parallelism**

(a) Differentiate between _data parallelism_ and _task parallelism_. 
(b) Explain how poor usage of locking mechanisms can lead to a deadlock. Identify the four necessary conditions for a deadlock to occur. 
(c) Briefly explain Amdahl's Law and its primary implication for multithreading and parallel scaling. 
(d) Compare alternative multithreading/parallelism approaches: OpenMP, Message Passing Interface (MPI), and OpenCL.
### **6. I/O Management**

(a) Differentiate between the three primary Linux device types: character, block, and net. (b) Explain the role of device drivers in presenting a standard interface to the OS, despite varying underlying hardware. 
(c) Contrast the user-space I/O commands: blocking, non-blocking, and Asynchronous I/O.
### **7. Filesystems**

(a) Define a filesystem and list its four primary requirements (persistence, organization, metadata, protection). 
(b) Explain how directories function as files containing nodes of information, and compare hierarchical (tree) directory structures with general-graph directory structures. Why does a general-graph structure require garbage collection? 
(c) Understand how Linux iNodes and hierarchical indirection (single, double, and triple indirect blocks) support the mapping of vastly large file sizes.
### **8. Networks**

(a) Explain why "Everything is not a file" when dealing with network sockets, specifically contrasting streams vs datagrams.
(b) In network programming, why must data be explicitly converted between network order and host order? 
(c) Describe the architecture of eBPF. How do the eBPF Verifier and JIT compiler allow developers to safely run custom code inside the Linux kernel?
### **9. System Security**

(a) Discuss the Principle of Least Privilege and explain how permissions can limit damage through compartmentalization. 
(b) Explain the concept of Protection Rings (e.g., Ring 0 vs Ring 3) and how they establish hardware-enforced domains of protection. 
(c) Inspecting C code that utilizes standard string manipulation (like `strcpy`), explain exactly how a buffer overflow attack is executed, what happens to the return address, and the ultimate consequence for the system.

---

**END OF QUESTION PAPER**