**VENETAS VITA** **University of Glasgow**

**OPERATING SYSTEMS (H)** **COMPSCI4011**

**COMPREHENSIVE REVISION PAPER**

**RUBRIC: Answer All 5 Questions** **This examination paper is worth a total of 60 marks.**

---

### **1. Processes and Scheduling (12 Marks)**

(a) Define a process and list the five standard lifecycle states it can transition through. [3]

(b) Differentiate between an OS process and a thread. Explain how threads provide a programming abstraction for execution. [3]

(c) Detail the exact steps of a context switch, specifically mentioning the role of the Program Counter (PC), Stack Pointer (SP), registers, and the Process Control Block (PCB) or Thread Control Block (TCB). [3]

(d) Contrast a First-Come, First-Served (FCFS) scheduling policy with a Round Robin (RR) scheduling policy. Why is it critical to set the RR scheduling quantum to an appropriate value? [3]

---

### **2. Memory Management (12 Marks)**

(a) Explain the fundamental trade-off between memory size and access times, and briefly describe how the OS tries to amortize this trade-off. [2]

(b) Virtual memory eliminates external fragmentation but introduces internal fragmentation. Explain why this happens, making reference to page and frame sizes. [3]

(c) Describe the page-table walk process for logical-to-physical address translation. Include the role of the Translation Look-aside Buffer (TLB) and how it minimizes effective access time. [4]

(d) Explain the phenomenon of _thrashing_. What is the worst-case memory scenario resulting from this, and how does the Out-of-Memory (OOM) Killer intervene? [3]

---

### **3. Concurrency and Parallelism (11 Marks)**

(a) Concurrency and parallelism are often confused. Define both terms and explain the difference between _data parallelism_ and _task parallelism_. [3]

(b) Define a _critical region_. Why is serialized access necessary when multiple threads attempt to interact with a shared resource? [2]

(c) Identify the four necessary conditions for a deadlock to occur in a concurrent system. [4]

(d) Briefly compare user-space synchronization mechanisms (like standard mutexes and semaphores) with kernel-space mechanisms (like spin locks and futexes). [2]

---

### **4. Filesystems and I/O (13 Marks)**

(a) The Linux operating system heavily relies on the "Everything is a file" abstraction. Describe this concept and provide two benefits it offers to the OS and developers. [3]

(b) Compare and contrast the following methods of mapping files to physical blocks on a storage medium: _contiguous_, _linked_, and _indexed_. [6]

(c) In the context of device management, contrast the user-space I/O commands of _blocking I/O_, _non-blocking I/O_, and _Asynchronous I/O_. [4]

---

### **5. Networks and System Security (12 Marks)**

(a) In a distributed system, how does a Local Area Network (LAN) differ from a Wide Area Network (WAN)? [2]

(b) In Linux network programming, data must often be converted between "network order" and "host order." Briefly explain why this is necessary and how socket buffers help optimize networking operations in the kernel. [3]

(c) Discuss the concept of _Protection Rings_ in system security. How do they establish domains of protection for hardware and software objects? [3]

(d) Explain the mechanism of a buffer overflow attack using C strings. What might happen as a consequence of a successful overflow, and how can an operating system mitigate it? [4]

---

**END OF QUESTION PAPER**