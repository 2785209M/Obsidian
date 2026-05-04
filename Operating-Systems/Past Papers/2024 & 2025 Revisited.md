# 2024
## 1. Process Scheduling
(a)**2/2** 
How does the round robin (RR) scheduling algorithm rely on timer interrupts? [2]
	The Round Robin Scheduling relies on timer interrupts from the hardware to enforce its set time quantum. Hardware interrupts force the CPU to initiate a context switch, storing the data from the current operation and loading data for the next operation in the queue.

(b) **3/3**
Why is it important to set the RR scheduling quantum to an appropriate value? [3]
	Setting the time quantum to a too-small value would increase the CPU overhead due to a higher frequency of context switches. Setting the time quantum to a too-large value would negate the usefulness of the algorithm, reducing concurrency and turning it into more of a first come first served approach. By rule of thumb the time quantum should allow approximately 80% of tasks to finish in one CPU burst (one time quantum).

(c) **2/2**
What factors might you take into consideration when setting the quantum? [2]
	I would take into consideration the burst time of the tasks. The time quantum should be large enough to allow 80% of tasks to finish in one burt but not larger.
	The time quantum should also be large enough that it makes the time it takes to perform context switches negligible

(d) **6/6**
Consider the following schedule of tasks, each with a CPU burst time and an arrival
time. (When new tasks arrive, they are placed at the end of the run queue.) Construct
a valid round robin schedule with a time quantum of 3 time units, and calculate the mean
turnaround time across all tasks. [6]
![[image.png]]
	-
	P0->P1->P2->P3
	-
	burst 1: P0 and P1 arrive at t = 0. The queue is [P0, P1]. P2 arrives at t = 1. The queue is [P0, P1, P2].  P0 executes for three clock cycles and has one remaining. The queue is [P1, P2, P0].
	burst 2: P3 arrives at t= 4. The queue is [P1, P2, P0, P3]. P1 executes for 3 clock cycles and has 3 remaining. The queue is [P2, P0, P3, P1].
	burst 3: P2 executes for one clock cycle and finishes at t = 7.  The queue is [P0, P3, P1].
	burst 4: P0 executes for one clock cycle and finishes at t = 8. The queue is [P3, P1].
	burst 5: P3 executes for 3 clock cycles and has 2 remaining. The queue is [P1, P3].
	burst 6: P1 executes for 3 clock cycles and finishes at t = 14. The queue is [P3].
	burst 7: P3 executes for 2 clock cycles and finishes at t = 16. The queue is empty.
	-
	P0 = 8
	P1 = 14
	P2 = 7 **- 1 = 6**
	P3 = 16 **- 4 = 12**
	
$\frac{8+14+6+12}{4} = 10 time units$

(f) **4/4**  
What kind of scheduling algorithm would you deploy for an OS on a multi-processor
system? Justify your answer. (Your answer should be no more than 200 words.) [4]
	I would deploy symmetric multiprocessing. I would have high priority processors and low priority processors operating on a shared queue. The high priority processors would use a priority-based scheduling algorithm such as Earliest Deadline First in order to ensure important tasks are finished within their time limit. The lower priority processors would use pre-emptive scheduling such as Shortest Job First in order to minimize waiting time and maximize average turnover time across non-real-time tasks. This design maximises throughput by reducing context switching and simulatneously optimizes for hard and soft deadline tasks.
## 2. Memory Mangement
(a)**3/3** 
Briefly discuss the merits of using virtual addressing instead of physical addressing, in a
computing system. [3]
	Virtual addressing allows processes to run using non-contiguous memory as virtual memory shows them an illusion of a contiguous address space to operate within.
	This address space is unique to each process and cannot be directly accessed by other processes, which eliminates the risk of cross-process interference.
	Finally, virtual addressing allows you to use virtual memory, which is when infrequently used data can be temporarily swapped out of RAM into a dedicated virtual swap space to make room for other data in memory, which allows programs to run that require more RAM than is actually available.
```
The term for unique address spaces is process isolation. This is the key benefit. This is also what leads to the processes thinking they have a contiguous address space.
```

(b)**2/2** 
What is the problem if the page size is set too small? [2]
	Page size being too small increases the rate of page faults and TLB misses, which slows down your system. It also increases the amount of time it takes to search through a page table as it makes the page table larger.

(f) **3/3**
What is the key advantage of multi-level page tables? [3]
	Multi level page tables allow the use of sparce address spaces, which means that if the page table isn't being fully utilized it can leave pages unallocated so as to not take up space in memory.
	Multi-level page tables also allow significantly faster lookup than flat page tables, which reduces the time taken for a TLB miss.

(g)**2/2**
In general terms, what are the purposes of the various page metadata bits stored in the
page table entries? (Your answer should be no more than 100 words.) [2]
	The metadata bits in page table entries provide important information about the pages. This includes: the dirty bit, the access bit, and the present bit.
	The dirty bit: indicates if a page has been altered since being loaded into memory, and if so must be saved before being evicted.
	The present bit: Indicates if a page is currently loaded in memory.
	The access bit: Indicates that a page was accessed recently
## 3. System Security
(a) **9/9** 
Discuss three denial-of-service (DoS) attack techniques, which could be local or remote,
for a multi-user shared server machine; explain how an operating system would mitigate
each DoS attack. [9]
	Fork() attack:
	A fork attack is when someone executes an infinitely multiplying program on your machine, typically done using a recursive fork() command which tells a process to infinitely clone itself. This will continue until your machine runs out of memory and crashes.
	This attack is mitigated in operating systems by setting limits on the amount of processes any given user can have in execution at a time. This is done in Linux by using the ulimit command. If it is exceeded then the process will be automatically shut down by the operating system.
	-
	SYN overload:
	A SYN overload attack is when someone sends an overwhelming number of SYN requests to a server, typically using a distributed network of infected hosts or spoofed IPs. The server will respond with a SYN+ACK, but the hosts will never send back an ACK, which leaves the handshakes half open. This will happen until the server's connection queue is full and no more hosts can connect, even if they are legitimate.
	This can be mitigated by the server's OS by using SYN cookies. Using TCP cookies, when the server sends its SYN+ACK back to a requesting host, it will bundle the TCP connection data into the packet. Once it sends this packet away it closes the connection so no half-open handshake is left. If the host is legitimate they will send back an ACK alongside the connection data and the server will re-establish a full connection.
	-
	Replay attack:
	A replay attack is when a hacker listens in to the conversation between a host and a server. The hacker could record one of the requests being sent to the server, save it, and replay it to the server later without needing to decrypt it. This could lead to the server executing a repeat instruction from the host. For example, if the request was a bank transaction it would result in the money being sent twice.
	This can be mitigated by using a TCP counter in the packet headers. The counter increments for every packet sent. If a packet is sent out of order it will be dropped by the server. The packet that was recorded by the hacker would be invalid to replay because the counter would not be in the right order.
# 2025
## 1. Memory Management
(a)**3/3** 
What are three challenges associated with exclusively using physical addressing? [3]
	Contiguous address spaces: Processes require their address spaces to be contiguous in order to work. This is difficult when using just physical addressing because you need to find a block of free physical memory large enough to hold the entire process.
	No virtual memory: The system will not be able to dynamically swap data out of RAM to make space for new data. This limits the size and number of the processes that can run in the system.
	Non-private address spaces: Address spaces in physical memory are susceptible to interference by other programs as they are unprotected.

(c)**3/3**  
What is temporal locality and why is it significant in virtual memory? [3]
	Temporal locality is the idea that an area of memory that was recently accessed is highly likely to be accessed again in the near future. This is significant in virtual memory as it is the basis for the page addresses that are stored in the translation lookaside buffer and allow for faster lookup of pages in page tables without having to rely on the MMU for full traversal every time.

(e)**4/4**  
What are the steps in a page table lookup when a TLB is present? [4]
1. The CPU request data from the MMU **at a virtual memory address**
2. The MMU checks the TLB for the page address 
3. TLB hit: The page address is in the TLB, the MMU gets the **Physical Address** instantly and immediately hands it back to the CPU **which immediately accesses the physical RAM**
4. TLB miss: The MMU has to traverse the page table and look for the address manually which takes much longer.

(f)**2/2**  
Consider a system with a large amount of RAM and the OS swappiness is set to 100.
What phenomenon will likely occur and how would you improve the situation? [2]
	Thrashing would occur. 100 is the largest possible swappiness value that can be set. For a system with a large amount of RAM this is unnecessary and would result in a huge amount of overhead due to frequent context switching. The swappiness level should be kept to the minimum level so that processes can fill up the available space in RAM before the OS starts swapping memory out into swap space.

(g))**4/4**  
Consider a system with an associative lookup time of 3ms and a memory cycle time of
30ms. Justify the role of the TLB making reference to the effective access times for a hit
ratio of α = 0.5. [4]
$EAT = (τ + ε)α + 2τ (1 − α)$
$EAT = (30+3)*0.5 + 2*30*(1-0.5)$
$EAT = 33*0.5 + 30$
$EAT = 46.5ms$
## 2. Concurrency and Parallelism
(e)**1/1** 
What is a critical region? [1]
A critical region is an are of code that allows threads to access shared resources.

(g)**4/4**
Consider a thread pool implementation where multiple worker threads process tasks from
a shared task queue. Each worker thread should access the queue sequentially, but mul-
tiple worker threads should be allowed to run simultaneously. Which synchronization
primitive(s) should be used, and why? [4]
	A mutex should be used. The mutex would lock the queue when a thread is accessing it and unlock it once it is done. This would allow for the threads to run simultaneously whilst protecting the cricital region. A mutex would be preferrable due to its simplicity and reliability compared to semaphores and barriers.
	You have to use a condition variable to notify other threads when a new task is added to the queue. When a thread is finished with its task and sees there are no more tasks to be done it would call pthread_cond_wait() to put itself to sleep and unlock the mutex. Then a producer can grab the mutex when it's ready and add a task to the queue, calling pthread_cond_signal() to wake up a consumer thread to do the work.
## 3. File Systems & I/O
(a) **3/3**
Name and describe 3 attributes or metadata that an OS will maintain for a file. [3]
	Name: The name of the file in a human readable format
	Size: The amount of memory the file takes up
	Creation Date: A timestamp for the date and time the file was created
```
You should mention the read/write bit.
```
(c)**3/3** 
What is a directory and give two reasons why they are used? [3]
	A directory is a symbol table that maps human-readable file names to their locations in physical memory. They are used for organization and to prevent naming conflicts.

(f)**3/3**
Consider a system with 4 byte pointers and a filesystem that uses a block size of 4KB and
single-level index block, what is the maximum file size that can be stored? [3]
	4KB = 2^12 
	4B = 2^2
	2^12 / 2^ 2 = 2^10
	2^10 * 2^12 = 2^22 = 4MB

(g)**2/2** 
Suppose that you want to modify the filesystem to enable a maximum file size of 4TB
without changing the block size, what would you change? Justify your answer. [2]
	You would change the block structure to a multi-level index block. Specifically a triple indirect index block. This allows for index blocks to point to index blocks which point to index blocks which point to data blocks. This exponentially increases how many adresses can be addressed.