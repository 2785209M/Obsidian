# Question 1 - Memory Management
(a) **1/3**
One challenge of using physical addressing is that data must be physically next to other data it's related to. For example the registers holding a large string would have to be physically next to each other in memory. 
Another challenge is the efficient use of memory. To avoid corrupting data you must set aside a large portion of empty data to ensure you can physically organize it properly. This wastes a lot of space.
Finally, physical memory takes longer to look up than virtual memory. It is represented as a flat plane rather than nested tables, which takes a long time to traverse to find data.
```
Lack of Memory Protection: 
Processes can potentially overwrite or read the memory of other processes or the OS itself, leading to crashes and security vulnerabilities.

Relocation due to contiguity constraints: 
Programs must be in contiguous blocks of physical memory. This makes concurrent process management difficult and leads to external fragmentation.

Process Size Limitations:
The size of a program is strictly limited by the amount of physical RAM available. There is no mechanism for swapping oarts of a process to disk transparantly (No Virtual Memory)
```

(b) **0/2**
Implementing a Memory Management Unit would mean you are able to use virtual memory. The MMU sorts physical data into virtual memory and maps each physical memory address to a virtual memory address.
```
The main effect would be a massive degradation in performance. The MMU is responsible for translating virtual addresses to physical addresses for every memory access. Doing this in software would require multiple instructions for every read and write, drastically slowing down execution.
```

(c) **0/3**
I have NO idea.
Temporary locality is the idea that virtual data can easily be rearranged from one place in virtual memory to another. This is significant because it means virtual memory doesn't have to be concurrent.
```
Temporary locality is the idea that if a specific memory location is accessed, it is highly likely to be accessed again in the near future. It is significant in Virtual Memory because it justifies the use of a translation lookaside buffer and demand paging. By keeping recently used page translations in the TLB and actively used pages in RAM, the OS minimizes page faults and TLB misses, making virtual memory perform at speeds close to physical memory.
```

(d) **1/2**
Fragmentation can occur in Virtual Memory. This can occur if a page is too large or to small. Fragmentation is the corruption of data.
```
The use of Virtual Memory completely eliminates External Fragmentation as contiguous virtual memory addresses can be mapped to non-contiguous physical memory addresses. However, you still risk Internal Fragmentation which is when a process allocates memory that doesn't perfectly align with the fixed page size (leaving wasted space inside the last allocated page frame)
```

(e) **0/4**
The stages of page table lookup with a TLB are:
1 - Look at the data that it is looking for
2 - scan the virtual memory space for that data
3 - return the data
```
1. The CPU generates a virtual address
2. The memory management hardware extracts the page number and checks the TLB
3. TLB HIT: If the page number is found in the TLB, the corresponding frame number is retrieved immediately.
4. TLB MISS: If not found, the system accesses the page table in main memory to find the frame number. The TLB is then updated with this new mapping for future use. The frame number is combined wth the offset to form the physical address.
```

(f) **1/2 - Poorly Explained**
This would most likely cause thrashing, which is the name given to what happens when the OS spends all of its time loading and storing data and doesn't have time to perform the actual processes it's trying to complete.
```
A swappiness value of 100 would cause the CPU to rapidly, aggressively swap out application pages into swap space in order to reclaim RAM, creating a high I/O overhead and potentially leading to thrashing.
A better method would be to set the swappiness value to 10 or 20, which would allow to OS to prioritize keeping application memory in RAM and only resort to swapping when physical memory is actually scarce.
```

(g) **0/4**
What
>	$EAT=(t_{lookup}+t_{memory})*\alpha+(t_{lookup}+2t_{memory})(1-\alpha)$ 
>	$EAT = (3+30)*0.5+2(3+30)(1-0.5)$
>	$EAT=48ms$ 
>	Without a TLB, every memory lookup requires two cycles. One for page table one for data. This would result in an access time of 60ms.
>	Even with a very low hit ratio of 50%, the TLB would reduce the EAT to 48ms. This demonstrates how TLBs play a critical role in accelerating address translation.

# Question 2 - Concurrency and Parallelism
(a) **3/4**
Concurrency is the execution of tasks one after another in one processor.
Parallelism is the utilization of multiple processors or "cores" to perform multiple tasks at the same time.
They are different from a programmer's perspective in terms of CPU utilization and processor throughput. 
```
Concurrency:
The illusion of parallelism by interleaving tasks on a single processor (process scheduling) in an independently executing process.

Parallelism:
The simultaneous execution of multiple tasks on separate processors (cores).

Programmer's Perspective:
Concurrency is about structuring a program to deal with multiple things at once.
Parallelism is about executing the program faster by utilizing hardware.
You can have concurrency without parallelism, but designing for concurrency often allows you to exploit hardware parallelism safely.
```

(b) **2/2**
On a single core CPU threads have to be executed one after another. This means that using multiple threads is actually less efficient than just using one concurrent process, as creating and destroying threads takes time.
On a multi-core CPU using multiple cores would increase the speed of a large program which handles a lot of data. But in a small program it may still be simpler and faster to not use threads at all, because thread creation and destruction would be a significant part of the process' lifetime.
```
Single-Core CPU:
Introducing threads would likely degrade performance as it would introduce overhead with no overlapping benefits. Context switching between the threads would be costly.

Multi-Core CPU:
Ideally the performance would improve linearly with the number of available cores up to the number of threads. Ideally performance would improve significantly. However with small programs the overhead introduced by creating threads could still slow the program down, and the proportion of a program that is serial would also limit the effectiveness of parallel processing (Amdahl's law).
```

(c) **1/1**
A race condition is what occurs when multiple threads try to manipulate the same data at the same time. Best case you get the wrong output from your function, worst case your program crashes.
```
Parallel:
Two threads accessing and manipulating data at the same time

Concurrent:
Two thread's processes being interleaved and accessing/manipulating the same data one after another in an unpredictable order, resulting in an unpredictable output
```

(d) **6/6**
thread 1 completes before thread 2:
thread one completes and them thread 2 completes, one after the other, bringing the final value of x[0] to 5
thread 2 completes then thread 1 completes:
They complete one after the other bringing the final value to 5 again
thread 1 wins:
If each thread executes one line of code after the other, in turn, and **thread 2** starts, the final value of x[0] would be 4
thread 2 wins:
If each thread executes one line of code after the other, in turn, and **thread 1** starts, the final value of x[0] would be 6
x[0] = 4,5,6

(e) **0/1**
A critical region is a section of code that alters physical memory.
```
A critical region is a region of code where a thread can access and modify chared resources. To prevent race conditions no two threads should be able to execute within a shared critical region at the same time.
```

(f) **0/2 - A mutex would be too costly**
Mutex. This would prevent race conditions.
```
A spinlock would be the most appropriate. This means threads wouldn't have to go to sleep and wait up like with a full mutex, which avoids overhead. The thread waiting for the lock would just busy-wait until the lock becomes available.
```

(g) **2/4 - Shockingly those are both correct answers**
Mutex. This would prevent race conditions. Wait maybe a semaphore. Whatever lets threads access a queue sequentially.
```
You would need to use a Mutex and a Semaphore:

Mutex:
Protects the Queue itself, forcing other threads to wait until the lock becomes available before they read or write to the queue.

Semaphore:
A semaphore would allow efficient management of thread state. Rather than busy-waiting this would allow a thread to go to sleep if the queue is empty and wake up when a producer adds a task.
```

# Question 3 - File Systems and I/O
(a) **2/3 - No Descriptions**
Creation date, Date last accessed, date last modified
```
Creation date: The exact date and time a file was created
Date last accessed: a timestamp for the last time the file was read
Data last modified: a timestamp for the last time the file was written to
```
(b )**0/2**
It reduces the load placed on the Operating System. It also means everything is accessible by the user.
```
It provides a unified, consistend abstraction layer (The Virtual File System). It allows programs to use the same system calls from a standard library to interact with all files without needing specialized APIs.
```

(c) **1/3**
A directory is a file used to store other files. They are very useful for organization as you can place directories within directories to create a nested tree-like structure.
```
A directory is a special file used for translating human-readable files into control blocks (like inodes) and stores where the files are stored on the disk.

Reasons for use:

Organization:
Allows users to organize their files logically for easier management

Naming:
Allows multiple files to have the same name in a file system, providing they are in different directories
```

(d) **0/1**
A two-level directory stores all of its data in one space rather than in nested directories, which makes it large and hard to navigate. Tree-structured directories organize their data in nested directories which are easier to navigate.
```
A two level directory restricts a system to a Master File Directory and individual User directories, where users cannot create subdirectories.
In a tree-structured direcctory users can create their own nested subdirectories.
```

(e) **0/3**
I have no idea what that means
```
An acyclic graph allows directories to share files and subdirectories via links but strictly forbids loops. A general graph allows loops.

If a graph has cycles, traversal algorithms can get stuck in infinite loops. avoiding this can be highly complex. Acyclic graphs remove this problem which is why they are preferred.
```

(f) **0/3 - Wrong but close somehow**
4x8=32b. 2^32=~4.3MB.  That is the max
>	$Block size = 4KB, Pointer Size = 4B$
>	$4096/4=1024pointers$
>	$1024*4KB=4MB$

(g) **0/2**
I would increase the size of the pointers. It is an exponential relationship so increasing the size of the pointers by not much results in a massive increase in the amount of memory you can address.
```
Implement a Multi-level index structure using indirect pointers.
This would allow you to exponentially increase the size of your filesystem by getting index blocks to point to other index blocks to point to data blocks.
```

(h) **0/3 - Completely off track**
I would choose tree-structured files. This is easier to navigate so it would require less processing power from the device. It would also be easier to maintain for developers. This is superior to two-level structure which would take longer to navigate.
```
Because of the limited resources I would choose a lightweight flash-optimized filesystem like littlefs. This is designed specifically for flash memory and provides protection against unexpected power loss which is common in embedded systems. It would be ideal because embedded systems typically lack to processing power to handle larger filesystems like ext4.
```