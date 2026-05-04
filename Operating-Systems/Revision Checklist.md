## Talking to the hardware

- [ ] Explain the boot sequence, from button press to OS control.
    
- [ ] Define interrupts and how they enable hardware/software communication.
    
- [ ] Describe how interrupt service routines (ISR) handle hardware signals.
    
- [ ] Explain the mechanism of building timers from interrupts.
    
- [x] Understand how user applications use system calls to interact with the OS.
    

---

## Processes

- [ ] Define a process and explain the 1 program to many processes relationship.
    
- [ ] Describe an OS process as a unit of scheduling, ownership, and administration.
    
- [x] Identify and define the process lifecycle states (running, sleeping, waiting, stopped, zombie).
    
- [ ] Review the system calls used for process lifecycle management (fork, exec, wait, exit).
    
- [ ] List the metadata stored within a Process Control Block (PCB).
    
- [x] Differentiate between a process and a thread.
    
- [x] Explain the mechanism of a context switch.
    
- [x] Contrast a context switch (mechanism) with a scheduling algorithm (policy).
    
- [ ] Describe how processes communicate using signals.
    

---

## Process Scheduling

- [x] Explain how high-frequency context switching creates the illusion of simultaneous execution.
    
- [x] Understand how scheduling moves processes between ready, running, and waiting states.
    
- [x] Differentiate between CPU bursts and IO bursts.
    
- [x] Detail the exact steps of a context switch involving the PC, SP, registers, and the PCB/TCB.
    
- [x] Compare pre-emptive and non-preemptive scheduling.
    
- [x] Define key scheduling metrics: CPU utilisation, throughput, load average, turnaround time, waiting time, and response time.
    
- [x] Analyze and compare scheduling policies: FCFS, RR, SJF, SRTF, HPF, EDF.
    
- [x] Explain the limitations of scheduling policies that require perfect information about the future.
    
- [x] Describe how priorities and user-level "niceness" affect process scheduling.
    

---

## Memory Management

- [x] Explain the fundamental trade-off between memory size and access times.
    
- [x] Compare bare-metal memory access with the benefits of Virtual Memory (VM).
    
- [x] Describe the role of the Memory Management Unit (MMU) in logical to physical address translation.
    
- [ ] Review memory allocation mechanisms (malloc/new, brk/sbrk, mmap).
    
- [x] Differentiate between internal and external fragmentation.
    
- [x] Define VM pages and physical memory frames, and discuss the trade-offs in their sizes.
    
- [x] Explain the page-table walk process for address translation.
    
- [x] Understand how hierarchies of page tables help scale accessible addresses.
    
- [x] Explain how the Translation Look-aside Buffer (TLB) acts as a cache to minimize access time.
    
- [ ] Identify the metadata stored in pages for OS administration and protection.
    
- [x] Define a page fault and describe the process of swapping.
    
- [ ] Differentiate between a working set and a resident set, referencing temporal locality.
    
- [x] Compare page eviction policies: NRU, LRU, clock, clock II.
    
- [ ] Explain the concepts of thrashing and demand paging.
    
- [x] Describe the worst-case memory scenario and the role of the Out-of-Memory (OOM) Killer.
    

---

## Concurrency and Parallelism

- [ ] Differentiate between concurrency (workload property) and parallelism (system property).
    
- [ ] Compare data parallelism with task parallelism.
    
- [ ] Explain how threads provide a programming abstraction for execution.
    
- [ ] Define race conditions, critical regions, and the necessity of serialized access.
    
- [ ] Explain locking mechanisms and how poor usage leads to deadlock.
    
- [ ] Identify the four conditions for deadlock and how the OS prevents or recovers from it.
    
- [ ] Compare user-space synchronization (mutex, semaphore) with kernel-space mechanisms (atomics, spin locks, futex).
    
- [ ] Understand Amdahl’s Law and its implications for multithreading.
    
- [ ] Review the implementation of POSIX threading in C (pthread_create, pthread_exit, pthread_join, sync primitives).
    
- [ ] Differentiate alternative multithreading approaches: OpenMP, Message Passing Interface (MPI), and OpenCL.
    

---

## IO

- [ ] Describe the primary categories of I/O management (storage, transmission, human-interface).
    
- [ ] Define the hardware components of IO (ports, bus, controller, mmap IO, registers).
    
- [ ] Differentiate between Linux device types: character, block, and net.
    
- [ ] Explain the role of device drivers in presenting a standard interface.
    
- [ ] Compare device management strategies: polling, interrupts, and direct memory access (DMA).
    
- [ ] Review code examples for registering and handling interrupts and device drivers.
    
- [ ] Explain why buffers are used to reduce context switch overhead during IO operations.
    
- [ ] Contrast user-space IO commands: blocking, non-blocking, and Asynchronous IO.
    
- [ ] Describe how DMA accelerates data movement.
    

---

## Filesystem

- [ ] Define a filesystem and its requirements (persLRUistence, organization, metadata, protection).
    
- [x] Explain the "Everything is a file" abstraction and its benefits.
    
- [ ] Describe file attributes, metadata, and how directories function as files.
    
- [ ] Review file lifecycle actions, file locking, and how the OS manages open files.
    
- [ ] Differentiate between sequential and direct file access methods.
    
- [ ] Explain the concept of file system partitions and volumes across storage devices.
    
- [ ] Compare directory organizations (hierarchies vs. graphs) and understand when garbage collection is required.
    
- [ ] Detail how files are protected using access groups and permissions.
    
- [ ] Explain the translation of logical block addresses to physical blocks.
    
- [ ] Compare the methods of mapping files to blocks: contiguous, linked, and indexed.
    
- [ ] Understand how Linux iNodes and hierarchical indirection support large file sizes.
    
- [ ] Describe the role of free space managers in storage mediums.