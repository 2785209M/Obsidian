# Memory Management
(a)**2/3** 
What are three challenges associated with exclusively using physical addressing? [3]
	If you are exclusively using physical addressing then your processes share an address space. This means that if you're not careful, your processes could interfere with each other's address spaces.
	You also have to be careful about keeping your memory blocks contiguous. Processes need their memory blocks to be contiguous for them to function.
	You have to be careful when transferring data between devices, e.g. to an external hard drive. Since the physical data isn't mapped it is easily corrupted if transferred out of order.
```
The third point is incorrect. The third challenge is being unable to use virtual memory to swap memory out of RAM to make more RAM available.
```

(b)**2/2** 
What would be the main effect of implementing the memory management unit in soft-
ware? [2]
	Implementing the memory management unit in software would cause a lot of overhead and slow your system down.

(c))**0/3**  
What is temporal locality and why is it significant in virtual memory? [3]
	Temporary locality is the idea that data can be easily transferred from one location in memory to another.
```
This is wrong. Temporary locality is the idea that if a location in memory is accessed, it is highly likely to be accessed again in the near future.
```

(d))**2/2**  
What is the significance of virtual memory on memory fragmentation? [2]
	Virtual memory eliminates external fragmentation as mapping means that processes will see their virtual memory as being contiguous even if their physical memory is not.
	However, virtual memory is still susceptible to internal fragmentation if a the pages are too large for most processes to fill their pages.

(e))**1/4**  
What are the steps in a page table lookup when a TLB is present? [4]
	1. The CPU needs data
	2. The CPU checks the TLB if it contains the page it is looking for
	3. If it does, the CPU stops looking and uses that page
	4. If it does not then the MMU searches the page table manually which takes a long time
```
This is kind of right but nowhere near detailed enough.
1. The CPU requests data at a virtual memory address (the CPU only sees virtual memory)
2. The MMU receives the request and check the Translation Lookaside Buffer
3. TLB Hit: The VPN is found in the TLB and immediately translated to a physical address and the CPU immediately accesses the physical RAM
4. TLB Miss: The MMU traverses the Page Table to look for the VPN. Eventually the MMU finds it and gives it to the CPU. This takes a comparatively very long time.
```

(f)**1/2**  
Consider a system with a large amount of RAM and the OS swappiness is set to 100.
What phenomenon will likely occur and how would you improve the situation? [2]
	Having a swappiness value of 100 means that the OS will be constantly and aggressively moving data out of RAM and into swap space. With large RAM a swappiness of 100 is completely unnecessary (swappiness only goes from 0 to 100), and the CPU will spend all of its time performing context switches rather than processing data.
```
Mention Thrashing. It specifically asks for what phenomenon would occur.
```

(g)**1/4**  
Consider a system with an associative lookup time of 3ms and a memory cycle time of
30ms. Justify the role of the TLB making reference to the effective access times for a hit
ratio of α = 0.5. [4]
	EAT = (30 + 3)a + 2 * 30(1-0.5) = 46.5ms

>$EAT=α(tTLB​+tmem​)+(1−α)(tTLB​+2tmem​)$
> $EAT=0.5(3+30)+0.5(3+30+30)$
> $EAT=0.5(33)+0.5(63)$
> $EAT = 48ms$ 
```
The effective access time is the time it takes to look up data from the TLB times the time it takes to look up data from the page table, multiplied by their respective rates. This is EAT=α(tTLB​+tmem​)+(1−α)(tTLB​+2tmem​).
```
# Concurrency and Parallelism
(a))**4/4**  
Define concurrency and parallelism and explain how they are different from a program-
mers perspective? [4]
	Concurrency is from the software's point of view, handling multiple tasks at the same time.
	Parallelism is from the hardware's point of view, processing multiple tasks at the same time
```
Concurrency is about dealing with lots of things at once, bu parallelism is about doing multiple things at once
```

(b))**2/2**  
How is the performance of a cpu-bound multi-threaded process effected by execution on
a single-core CPU vs a multi-core CPU? [2]
	a multi-threaded process would run significantly faster on a multi-core CPU than on a single core CPU. Since threads can run in parallel they would be able to execute simultaneously rather than concurrently.

(c))**1/1**  
In the context of a multi-threaded process, what is a race condition? [1]
	A race condition is when two threads are accessing/modifying a critical region at the same time.

(d)**6/6** 
Consider the following code of two threads running simultaneously:
```
// shared state
int x[1] = {5};
void thread_1(){
int R1 = x[0];
R1 = R1 + 1;
x[0] = R1;
}
void thread_2(){
int R2 = x[0];
R2 = R2 - 1;
x[0] = R2;
}
```
What are the possible final values of x[0] if no synchronisation is used? Show the
sequence of operations in each case. You may assume that each line statement within the
threads completes atomically. [6]
	x[0] = 4, 5, 6
	if each thread executes one after another the answer would be 5.
	If they both start before the other thread can modify the data, then it comes down to which thread finishes last; if thread 1 finishes last the value is 6, if thread 2 finished last the value is 4.

(e)**0/1** 
What is a critical region? [1]
A critical region is an editable area of shared memory that multiple threads have access to.
```
This is not quite right. A critical region is an area of code that accesses shared resources.
```

(f)**2/2**
Consider a shared counter that is incremented frequently by multiple threads within the
kernel. Which type of locking mechanism would be the most appropriate and why? [2]
	A spinlock would be the most appropriate. As the counter is incremented frequently it doesn't make sense to put the threads to sleep. instead the simplest way to prevent multiple threads from accessing the counter simultaneously would be tom make them busy-wait until the lock is free.

(g)**0/4**
Consider a thread pool implementation where multiple worker threads process tasks from
a shared task queue. Each worker thread should access the queue sequentially, but mul-
tiple worker threads should be allowed to run simultaneously. Which synchronization
primitive(s) should be used, and why? [4]
	The one where a program will execute until a certain point but then stop if the other thread hasn't finished its job yet.
```
This is a mutex combined with a condition variable or a semaphore. The mutex makes sure critical regions are mutually exclusive, and the CV/Semaphore signals the waiting threads when a new task is added to the queue.
```
# File systems and IO
(a) **2/3**
Name and describe 3 attributes or metadata that an OS will maintain for a file. [3]
	Date last Accessed: Stores a timestamp for the last time the file was accessed
	Date Last Modified: Stores a timestamp for the last time the file was modified
	Date Created: Stores a timestamp for the date and time the file was created
```
For variety, it is better to mention: permissions(read/write), owner ID, and file size.
```

(b)**2/2** 
Why is the Linux “everything as a file” approach significant for the OS? [2]
	It allows a standard library of functions to be used for all programs rather than having to use specialized software.

(c)**0/3** 
What is a directory and give two reasons why they are used? [3]
	A directory is a nested sequence of files. Directories are significantly easier to navigate than large files.
```
A directory is a symbol table that maps human-readable file names to their physical memory locations. It is used for logical organization and to prevent naming conflicts.
```

(d)**1/1** 
Describe the main difference between two-level directories and tree-structured directo-
ries? [1]
	A two-level directory doesn't allow users to create directories within the parent directory. Tree-structured directories give users the freedom to nest directories as much as they like.

(e)**3/3** 
How does a general-graph directory structure compared to an acyclic-graph directory
structure and why? [3]
	A general-graph structure allows for recursive loops in the file system. acyclic graph directory structures do not allow this, as it makes directory traversal algorithms extremely complex due to the possibility of getting stuck in an infinite loop.

(f)**0/3**
Consider a system with 4 byte pointers and a filesystem that uses a block size of 4KB and
single-level index block, what is the maximum file size that can be stored? [3]
```
In an indexed allocation system, and indexed block is a single block of memory filled entirely with pointers to the data blocks.
4KB = 4096 Bytes
1 pointer = 4 bytes
4096/4 = 1024 pointers
1024 x 4KB = 4MB
```

(g)**1/2** 
Suppose that you want to modify the filesystem to enable a maximum file size of 4TB
without changing the block size, what would you change? Justify your answer. [2]
	I would change the file system to be a tree-like structure rather than a single-level index. Tree-like structures allow for more data to be pointed to with the same amount of bits for the pointer.
```
The correct term is multi-level indexing.
```

(h)**3/3** 
You are asked to design a file system for a resource limited, embedded sensor device.
Which file system would you recommend and why? (your answer should be less than 100
words) [3]
	I would use a simple and small file system as embedded devices typically have limited processing power and memory. FAT32 would be a good choice, but there are more specialilzed options which could offer superior qualities for specific tasks. I would not choose a complex filesystem like ext4 as that could end up throttling the embedded device and reducing functionality.

# 36/60 = 60%