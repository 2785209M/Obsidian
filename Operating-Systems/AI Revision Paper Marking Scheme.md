### **Marking Scheme: OPERATING SYSTEMS (H) - COMPSCI4011**

#### **Question 1: Networks and I/O (20 Marks Total)**

**(a) What is the main difference between a Local Area Network (LAN) and a Wide Area Network (WAN) in the context of distributed systems? [2]**

- **1 mark:** A Local Area Network (LAN) connects computers and devices within a limited, localized area (such as a single building or campus).
- **1 mark:** A Wide Area Network (WAN) connects networks across broad geographical distances, allowing distributed systems to span cities or countries.

**(b) In Linux networking, why are socket buffers used when handling network data in the kernel? [2]**

- **2 marks:** Socket buffers are used specifically to reduce `memcpy()` operations within the kernel. By managing data in these buffers, the OS minimizes the overhead of copying data between different networking layers and buffers, significantly improving network performance.

**(c) Briefly describe how eBPF (Extended Berkeley Packet Filter) can be utilized within the Linux kernel. [3]**

- **1 mark:** eBPF allows developers to execute custom, sandboxed code directly within the Linux kernel.
- **1 mark:** It utilizes an eBPF Verifier and a JIT (Just-In-Time) Compiler to ensure the code executes safely and efficiently without crashing the kernel.
- **1 mark:** It is frequently used to interact directly with network components like sockets and the TCP/IP stack for monitoring, security, or performance routing.

**(d) Compare polling and interrupts as device management strategies. [4]**

- **2 marks (Polling):** Polling requires the CPU to repeatedly check the status of an I/O device to determine if it is ready for data transfer. This can waste valuable CPU cycles (busy-waiting) if the device is slow.
- **2 marks (Interrupts):** Interrupts allow the hardware device to send a specific signal to the CPU only when it requires attention or has completed a task. This frees up the CPU to perform other work in the meantime, making it much more efficient for system resources.

**(e) Explain how Direct Memory Access (DMA) accelerates data movement during I/O operations. [4]**

- **2 marks:** DMA accelerates data movement by allowing a dedicated DMA controller to handle the transfer of data directly between the I/O device and main memory.
- **2 marks:** Because the DMA controller manages the bulk of the transfer, the CPU is completely bypassed for the actual data movement and is only interrupted once the entire block of data has been successfully transferred.

**(f) Why are buffers utilized to reduce context switch overhead during I/O operations? [5]**

- **2 marks:** I/O operations are extremely slow compared to the execution speed of the CPU.
- **3 marks:** If a process had to wait (block) and wake up for every single byte of data transferred, it would trigger a massive number of context switches. Buffers allow data to be gathered into larger blocks; the process only needs to be context-switched when a buffer is full or empty, drastically reducing context switch overhead and improving throughput.
#### **Question 2: Hardware, Processes, and System Security (20 Marks Total)**

**(a) Describe the computer booting sequence, from a hardware button press to the operating system taking control. [4]**

- **1 mark:** The sequence begins with a physical hardware button press to power the system.
- **2 marks:** The CPU then jumps to a pre-defined memory location to execute the initial boot code.
- **1 mark:** Once the hardware is initialized, the boot code hands control over to the OS, and the OS begins its normal execution.

**(b) Differentiate between an OS process and a thread. [3]**

- **1.5 marks:** An OS process is defined as a program in execution and acts as the fundamental unit of scheduling, ownership, and administration.
- **1.5 marks:** A thread is a component within a process; processes can contain many threads, and these threads are the actual entities of execution that share the process's resources.

**(c) List three examples of metadata stored within a Process Control Block (PCB). [3]**

- _Award 1 mark for each correct example, up to a maximum of 3 marks._
- Process ID (PID).
- Parent process information.
- Process priority.
- Thread state.

**(d) Explain the Principle of Least Privilege and how it can limit potential damage within a system. [3]**

- **1 mark:** The principle states that permissions should be limited, either statically or dynamically, to only what is strictly necessary.
- **2 marks:** This limits damage by compartmentalizing system and data access. If a process or user is compromised, the malicious actor can only access or damage the specific compartments they have privilege for, rather than the entire OS.

**(e) Describe what a "fork bomb" is and state the mechanism an operating system can use to prevent this specific type of local denial-of-service attack. [4]**

- **2 marks:** A fork bomb is a type of local denial-of-service (DoS) attack where a process continually replicates itself (using the fork command) until system resources are completely exhausted.
- **2 marks:** An operating system can prevent this by implementing limits, specifically utilizing `ulimit` to restrict the number of processes a single user or application can spawn.

**(f) Explain the mechanism of a buffer overflow attack and how bounds checking can be used to mitigate it. [3]**

- **2 marks:** A buffer overflow attack occurs when a program reads or writes more data to a buffer than it was intended to hold, which can overwrite adjacent variables, alter return addresses, or inject malicious executable code.
- **1 mark:** Bounds checking mitigates this by actively padding and verifying that the size of the data being written does not exceed the allocated boundaries of the buffer before the execution happens.