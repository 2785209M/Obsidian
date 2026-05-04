# 1. Process Scheduling
(a) **2/2 - Mention the ready queue**
The Round Robin Scheduling Algorithm relies on timer interrupts to indicate at which time a context switch should occur. Round Robin is a timing based Scheduling algorithms in which each process gets an equal allotted time quantum, through which they are swapped sequentially until completing. The saved context for each process must be restored when they begin again and stored when they stop.

(b) **3/3**
It is important to set the Round Robin Scheduling quantum to an appropriate value for two main reasons: one is to prevent any individual process from taking up too much processing time from the other processes, possibly leading to not enough processes completing in the given time frame. The other is to make sure that the context switching itself doesn't take up too much of the time quantum. Too many context switches can lead to thrashing, which is when there isn't enough time to do anything except jump between context switches.

(c) **2/2 - context switching should be a negligible fraction of the time slice, Quantum should be large enough that 80% of processes complete in a single time slice**
I would take into consideration the turnaround time of the individual programs and the individual arrival times of the tasks to maximize the throughput by minimizing context switching and maximizing CPU utilization.

(d) **5/6 - Mean turnaround time is 10 not 11.5. You flubbed the calculation somehow**
The time quantum is 3 time units. I would set the Round Robin to start at P0 and work through to P3, then loop back around. P0 -> P1 -> P2 -> P3. It would take two cycles to complete every task. P0 = 11, P1 = 14, P2 = 6, P3 = 15. 11 + 14 + 7 + 19 = 51, 51/4 = 11.5 Average TAT.

(e) **3/3**
You could alternatively use the Shortest Job First Algorithm. It is not preemptive so it requires less context switching and it would complete shorter tasks first, reducing the average waiting time. The order of execution would be P0 -> P2 -> P3 -> P1. TAT: P0 = 4, P1 = **16**, P2 = 4, P3 = 10. 4 + 16 + 4 + 10 = 34. 34/4 = 8.5 average TAT.

(f) **4/4 - Having multiple ready queues also means threads don't have to wait so long for locks to be released**
This depends whether or not the OS I am using is Linux. If it is Linux I would utilize the Completely Fair Scheduler. If not, I would implement asymmetric multiprocessing. For asymmetric multiprocessing you need to run a separate instance of the OS on each core. Core 0 would handle soft deadline tasks while core 1 would handle hard real-time tasks. If one core hangs the other is uninterrupted. On core 1 I would implement Deadline monotonic scheduling to ensure tasks finish before their deadline. On core 0 I would employ first come first serve to minimize the context switching. This allows for efficient scheduling and prevents important tasks from being slowed down by menial tasks.

# 2. Memory Management
(a) **1/3**
Using virtual memory techniques such as pages tables can avoid external fragmentation as it means physical memory can be non contiguous (data doesn't have to be physically in the correct order to make sense). Otherwise data could be corrupted if it is moved incorrectly in physical memory - during data transfer to an external drive for example.
```
Having virtual memory addresses allow a process to think it has a large flat contiguous physical address space when really it's accessing non-contiguous physical addresses being translated virtually.
Every Process has an address space assigned to it which prevents processes from messing with other process' data.
Virtual Memory addressing allows data to be swapped in and out of the disk giving the illusion of having larger-than-actual RAM.
Processes can also map their virtual address space to the same physical frame which allows for efficient inter-process communication.
```
(b) **0/2**
Using pages that are too small would increase the memory overhead as you would have to spend more time performing context switches. This could lead to frequent page faults.
```
Having too-small pages means you have an enourmous number of pages per process. This forces the page tables to become excessively large which consumes physical memory just storing address mappings.
It also increases the frequency of Translation Lookaside Buffer (TLB) misses
```
(c) **2/2**
If the page size is too large, significant internal fragmentation can occur. If the processes being loaded into the page table are much smaller than the page itself, the excess page data will just sit taking up space in RAM that other processes could be using. Large pages also increase overhead as they take longer to switch in and out of RAM.

(d) **3/3**
To calculate the amount of pages in the system we first have to calculate the offset bits. Since each page can address 4KB, and 4KB = 4096B = 2^12B, each page requires 12 bits for the offset. We know that there are 20 bit addresses in total. To find the amount of bits set aside for the page number: 20-12 = 8B. 2^8 = 256, which means we have 256 pages in the system.
```
4KB = 2^12 = 12 bits offset
20 - 12 = 8 Bits for the Page Number
2^8 = 256 Pages
```

(e) **2/2**
The amount of second level pages is equal to two to the power of the number of top level pages which in this case is 4. 2^4 = 16 pages.
```
2^4 = 16 Page Numbers
Hierarchical tables increase exponentially with levels and bits added
```
(f)**3/3**
Multilevel tables provide greater space efficiency than low level tables. Hierarchical paging reduces the amount of space that a page table consumes. They also provide faster lookup in large databases than flat page tables. For a flat page table every bit needs to be stored in memory throughout its lifetime. For a hierarchical page table bits can be allocated in the second layer tables only if they are used, and simply marked as invalid if they are not. This saves huge amounts of RAM.
```
Multilevel page tables provide far greater space efficiency. They require less bits to address the same amount of data as a flat page table. They also allow for the dynamic allocation of memory. Multi-level page tables allow an OS to only allocate lower-level page tables for specific memory ranges that are actually in use, leaving others unmapped, which saves huge amounts of RAM.
```
(g) **2/2**
Page metadata contains information about page protection, sharing, and caching. The protection bit tells the OS whether a page is read/write or read only. The access bit stores data about the last time the page was accessed. The present bit shows whether the page is currently in physical memory or not. Metadata lives in the higher level bits within the page whereas the physical address data is stored in the lower bits.
```
Valid/Invalid Bit: Indicates if the page is currently loaded in Memory or not
read/Write Bit: Enforces Memory Protection
Dirty Bit: Tracks if a page has been modified since being written to the disk. If it has it's dirty and needs to be cleaned (written back to the disk) before being unloaded
Accessed Bit: Used by Page Replacement algorithms to determine which pages are frequently used
```
(h) **3/3**
We would need to use a hierarchical page table with a Large Page bit to tell the memory management unit how big the offset is supposed to be and stop the page walk. If we set the base offset to 12 we would get a base page level of 4KB. If we set our page sizes to 8 and make two more levels that gives us 2^20 = 1MB and 2^28 = 256MB for levels 2 and 3. If we set the LP bit to 1 in level 1 the page size will be 4KB. If we set the LP in level 2 the page size will be 1MB and so on. We would need a Transition lookaside buffer capable of handling multiple page sizes or simply use multiple TLBs.
```
Implementing a Huge Page Bit can short circuit the address translation process. 
If you implemented a 3 level table, you could set the addresses to be 40 bits long and set aside 12 bits for the base offset you would start with 4KB pages. If you implemented a Huge stop bit you could get the MMU to stop in a specific level and treat the page numbers beyond that level as extra offset, increasing the size of the pages. In this example we could split level 1 into 12 bits, level 2 into 8 bits, and level 3 into 8 bits. If you put the Huge stop bit in level 2 you would get 1MB pages. If you put it in Level 1 you would get 256MB pages. If you didn't put one at all you would still get 4KB pages.
```
# 3. System Security
(a) **3/9 - Listed But didn't Describe**
```
Fork attack:
A fork attack is when a local uses executes a recursive fork() command, which makes a process create infinite clones of itself. This starves the CPU and prevents anybody using the machine
OS mitigation: Operating systems enforce quotas on the maximum number of processes a user can create (ulimit in Linux)

Memory Exhaustion:
A user can execute a program that endlessly allocates memory in RAM without Freeing it. This could force the system to thrash and/or invoke the OOM Killer which might terminate important processes
OS Mitigation:
The OS sets an upper limit on the amount of physicall memory and swap space a single user can consume

TCP SYN Overload:
When a remote attacker repeatedly sends SYN requests to a server but never completes the TCP handshake, filling up the server's queue and preventing anybody else from connecting. This is typically done via a network of infected machines (DDoS)
OS Mitigation:
Servers use SYN Cookies. Instead of leaving connections half open, the OS encodes the connection information into the SYN-ACK and only reopens if the information is sent back in the ACK message.
```

(b) **3/4**
The issue lies in the datatype of len. len is set as a char, which can hold a maximum of 8 bits (-128 to 127 or 0 to 255). If the argv[1] string has > 255 characters, the number will wrap around. if the string is 257 characters long then len will become 2. This means that the if statement will return true and the process will try to copy argv[1] into the 100 character long buffer. This will make it overflow by 157 bytes and overwrite adjacent memory. 
```
The consequences of this are if the overflow overwrites the function's return pointer then an attacker could embed malicious code into the string, which could achieve execution when main() returns
```

(c) **7/7**
Tagging can be used to prevent buffer overflows and nullify the risk of dangling pointers being used maliciously. When a pointer is created is will have a few bits set aside to work as a tag, and the system automatically divides memory into blocks called granules which each also have a few bits set aside for a tag. Before accessing data the hardware will check whether the tags on the pointer and the granule being accessed match. If they do not match, this means the data has either been corrupted or the pointer is trying to access data outside of its scope. If this happens the hardware will throw an exception and the process will be terminated before any corruption can occur. This prevents data overflowing from a buffer as the system would throw an exception as soon as the pointer tried to access data outside the buffer. It would also prevent hackers using dangling pointers as when memory is freed its tag changes, so the dangling pointer would no longer be able to access the physical memory location it is pointing to.
```
The tag is allocated when a program allocates memory via malloc. The same tag is given to the pointer for this memory.
once memory is freed its tag is removed and once it is allocated to something else its tag is changed. This stops hackers from user dangling pointers to access the old memory.
The tag check is done by the hardware not the software.
```