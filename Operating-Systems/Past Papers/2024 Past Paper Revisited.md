## Process Scheduling
(a)**1/2** 
How does the round robin (RR) scheduling algorithm rely on timer interrupts? [2]
	Interrupts in the Round Robin scheduling algorithm determine the time quantum each task gets to execute per cycle.
```
The hardware timer imposes interrupts on the CPU, forcing it to give control back to the OS handler and perform a context switch.
```

(b) **1/3**
Why is it important to set the RR scheduling quantum to an appropriate value? [3]
	 This reduces the likelihood that tasks will be left unfinished. Having a time quantum too large would mean some tasks are neglected, but having time slices too small may mean short tasks don't finish on time. You should aim to keep the average turnaround time as low as possible.
```
Having a time quantum too large effectively turns the algorithm into FCFS. This gives it a high delay for interactive applications.
If the time quantum is too small, the CPU will suffer from high overhead due to the frequency of context switching, which reduces the system's throughput.
```

(c) **1/2**
What factors might you take into consideration when setting the quantum? [2]
	I would take into consideration the average burst time and turnaround time of the individual tasks. The Round Robin Scheduling algorithm should allow approximately 80% of tasks to complete on the first cycle.
```
You should also mention that the time quantum should be so large that it makes the time it takes to perform context switches negligible.
```

(d) **3/6**
Consider the following schedule of tasks, each with a CPU burst time and an arrival
time. (When new tasks arrive, they are placed at the end of the run queue.) Construct
a valid round robin schedule with a time quantum of 3 time units, and calculate the mean
turnaround time across all tasks. [6]
![[image.png]]
	PO->P1->P2->P3
	
	P2 will finish on the first cycle
	P0, P1, and P3 will finish on the second cycle
	
	P0: TT = 13
	P1: TT = 16
	P2: TT = 1
	P3: TT = 18
	
	average = 48/4 = **12**

```
P0: TT = 8
P1: TT = 14
P2: TT = 6
P3: TT = 12

average = 40/4 = 10 time units

When a task enters a ready queue it is automatically placed at the end.
```

(e) **3/3** 
Which alternative scheduling algorithm could be used to reduce the mean turnaround time
across all tasks? Explain why the alternative improves this metric. [3]
	Shortest job first would reduce the average turnaround time.  Performing the shortest jobs first significantly reduces the amount of time each program spends waiting before execution, which minimizes turnaround time.

(f) **2/4**  
What kind of scheduling algorithm would you deploy for an OS on a multi-processor
system? Justify your answer. (Your answer should be no more than 200 words.) [4]
	I would implement a dual scheduler across the multiple processors. I would use different scheduling algorithms in each and assign tasks to processors by priority. For the low priority processor (non real-time tasks) I would use the shortest job first algorithm to reduce turnaround time and overhead. On the high priority processor I would use Closest Deadline first to make sure tasks finish before their deadlines.
```
This is asymmetric multiprocessing. What you were trying to describe is symmetric multiprocessing. This is when the OS maintains either a global run queue or a queue per processor. It is important to mention load balancing between the processors, and Processor affinity - trying to keep a process on the same CPU to maximise cache hits.
```
## Memory Management

(a)**2/3** 
Briefly discuss the merits of using virtual addressing instead of physical addressing, in a
computing system. [3]
	Using virtual memory provides two main benefits: it allows physical memory to be non-contiguous and allows for virtual memory to be used to artificially extend RAM. This eliminates external fragmentation and allows for the system to run processes that require more RAM than it actually has. It does this by setting aside a long-term memory "swap space" to act as RAM. It will swap infrequently used process data from RAM into this store to free space for other data.
```
Third Key benefit: Process isolation. Using virtual addressing processes cannot access the memory of other processes as their virtual memories map to different physical frames. It provides a simplified programming model as every program thinks it has a contiguous block of memory.
```

(b)**1/2** 
What is the problem if the page size is set too small? [2]
	Setting the page size too small increases the amount of context switches required and increases memory overhead. It also increases the amount of bits that need to be set aside for metadata.
```
Context switches is not the right term. Smaller pages means more pages, which requires larger page tables which increases memory overhead. Having more pages also increases the rate of page faults and TLB misses.
```

(c) **2/2**
What is the problem if the page size is set too large? [2]
	Page size being too large increases internal Fragmentation. Pages should be just big enough so that 80% of processes can fit inside of one page.

(d)**3/3** 
Consider a system with 20 bit addresses and a page size of 4KB. How many bits would
be needed for the page number and how many pages would there be in the system? [3]
	4KB = 2^12 = 12 bit offset
	20-12 = 8 bit page number,  2^8 = 256 pages

(e) **2/2**
If we use a 2-level page table, with 4 bits for the top level entry, how many 2nd level page
tables will there be if all are in use? [2]
	2^4 = 16 tables

(f) **2/3**
What is the key advantage of multi-level page tables? [3]
	In multi-level page tables not every table has to be assigned. Tables can be left blank to save memory. It is also possible to dynamically change the size of multi-level page tables using the huge page bit.
```
To be more concise say "Supports sparce address spaces".
```

(g)**1/2**
In general terms, what are the purposes of the various page metadata bits stored in the
page table entries? (Your answer should be no more than 100 words.) [2]
	They are used to store data about the page tables. This includes time of last access, time of last modification, the location of the corresponding frame in physical memory, etc.
	The dirty bit: used if a page was modified whilst being loaded in memory. if set, the page must be saved before being removed from RAM.
	Access bit: records the time of last access
	Modified bit: records the time of last modification
```
Access bit: Set if the page has been accessed recently
Dirty Bit: Set if the page has been modified since being loaded into RAM and needs to be written back to the disk before being evicted
Present Bit: Set to 1 if the page is loaded in RAM
```

(h) **3/3**
How would you design a paging system that would allow pages with different sizes, for
example, you might have a system with small, medium and large pages being 4KB, 1MB
and 256MB respectively? (Your answer should be no more than 150 words.) [3]
	You would have to use a hierarchical page table with a huge page bit.
	If you want the base page size to be 4KB you need to set the base offset to be 12 bits. from there you can add 8 bits to the address per level for the page numbers. In this case we have three levels, so our total address size would be 38 bits. This is what will happen to the page number and offset if you set the huge page bit in the different levels:
	Level 1 (bottom): 12 bit offset, 16 bit page number -> 4KB, 64,000 pages
	Level 2 (Middle): 20 bit offset, 8 bit page number -> 1MB, 256 pages
	Level 3 (Top): 28 bit offset, 0 bit page number -> 256MB, 1 page

## System Security
(a) **6/9** 
Discuss three denial-of-service (DoS) attack techniques, which could be local or remote,
for a multi-user shared server machine; explain how an operating system would mitigate
each DoS attack. [9]
	Fork attack:
	A fork() attack is when a malicious user executes an infinitely replicating program on a machine, which eventually overwhelming the system's RAM and causing it to freeze or crash. The OS mitigates this kind of attack by implementing limits (ulimit in Linux) on how many processes a user can create at a time
	-
	SYN flood:
	A SYN flood attack it when a malicious user sends thousands of SYN messages to a server to initiate a TCP handshake, but never sends the ACK, leaving every handshake half-open. This fills up the server's connection queue and prevents anybody else from accessing the server. The server's OS mitigates by using SYN cookies. This is when, upon receiving a SYN request, it will bundle the connection information into its SYN+ACK, and then close the connection. If the client is legitimate they will send back an ACK with the bundle of connection information and the server will re-establish a full connection with the client.
	-
	Replay Attack:
	A replay attack is when a malicious host spoofs somebody else's IP address and sends a request for a huge amount of data to a server. This causes the server to send data to the innocent owner of the spoofed IP, which could overwhelm their machine causing it to freeze or crash. This is typically done when somebody is using TCP fast open or TLS 0-RTT.
```
That's not a replay attack. That's an amplification attack.
Replay attack: When a hacker intercepts a packet and re-transmits it again later causing an action to be repeated. This is mitigated by the OS using sequence numbers so keep track of how many packets have been sent between the client and server. The sequence number on the stolen packet would be old and invalid.
```

(b) **4/4**
Inspect the C code below. How could this code cause a buffer overflow, and what might
happen as a consequence? [4]
```
#include <stdio.h>
#include <string.h>
#define MAX 100
int main(int argc, char **argv) {
char len;
char buf[MAX];
len = strlen(argv[1]);
if (len < MAX) {
strcpy(buf, argv[1]);
}
return 0;
}
```
	This code could cause buffer overflow as it is typing len as char. A char is only 8 bits, and typically ranges from -128 to + 127. If the input string is greater than 127 (say 128), then the char will loop around the other way a become -127. since -127 < 100(MAX), the if statement will return true and the operation will go ahead. This could result in more data than the buffer can hold being copied onto the buffer, causing buffer overflow.

(c) **7/7**
Describe an approach to hardware memory tagging and explain how this might mitigate
memory-based exploits. You may provide pseudo-code examples if required. [7]
	Memory tagging can be used to prevent buffer overflow. This is when a few bytes in a pointer and the memory it points to are set aside to be used as a tag. Whenever a user tries to access memory with the pointer, the hardware will check whether the tag in the pointer matches the tag of the memory. If it does the operation will go ahead, but if it doesn't the hardware will throw an interrupt and stop the operation before any memory is corrupted.
```
You could mention ARM MTE (Memory Tagging Extension) which is a real life example
```

# 44/60 = 73.33%