# Managing Memory
It is possible for virtual address space to be much bigger than physical memory. If you try to access a page that does not have a corresponding frame in RAM you will encounter what is known as a **page fault**, which is a processor exception that must be handled by an OS exception handler.

A **Working Set** is the minimum required amount of pages for a process to make progress.

**In Memory Caches** are physical memory frames used by the OS to cache data.

**Page replacement policies** are Operating System algorithms used to decide which page to swap out when a new page needs to be loaded in. Examples include **random, clock, NRU, and LRU**.

**Swapping** is the process of moving unrequired pages from RAM into backing storage to free up memory. This is done through persistent storage or a file in /root called **Swap Space**. **Swapping**
is a key component of **Virtual Memory**.

A **Page Fault** occurs when you try to access a page in RAM that has been swapped into the **swap space**.

## Handling Page Faults
Page faults occur when a process fails to access a page. Once the page has been found / the fault has been resolved, the process will resume.
- First, the handler checks if the page is in the process' address space
- Then, it checks if the page supports the requested access mode
- Finally, if both conditions **are** met, the handler checks where the fastest place to read the data from is and performs the appropriate action to get the page data
- If the page address or requested access mode are invalid, a segfault is thrown.
- If the data is in swap space, a swap occurs
- If the page is resident but being used the data is copied using copy on write
- If the data is in backing storage, it gets read into a fresh empty page

In linux, there is a difference between major and minor faults.

A **Major Fault** is when data must be read from backing storage.
A **Minor Fault** is when data is present in RAM but has been mapped to a different process.

# In-memory Caches

### Page Cache
- Stores Files in page sized chunks after being first accessed
- OS looks ahead to find pages to load prior to usage
- Allows quicker access times after being previously accessed

### Buffer Cache
- Used to Optimize access block devices
- Interposes read and write accesses as they are costly to slow device blocks and increase latency

### Swap Cache
- Tracks **frames** that have written out to **Swap Files**
- When a page has been swapped out it will remain in swap file slot when swapped back in
- Swap file records file's location in swap to be put back into the page entry table
- If a page is modified, then swap is expunged and rewritten as it becomes dirty
- This functionality reduces unneccessary swap file writebacks

![[image-15.png]]


# Page Replacement Policies
There are 2 types of pages you should consider when swapping: **Clean** pages and **Dirty** pages.
- **Clean** pages can be discarded as they are already stored in the backing store
- **Dirty** Pages must have data transferred to the backing store before being swapped due to being updated since they were last pushed or were never pushed at all
It is inefficient to swap out pages that are needed as they will need to be swapped bak in soon after being swapped out.

## Random
**Randomly** selects pages to be swapped out

### Not Recently Used
When a process starts, the OS will set a **R**eferenced and **M**odified bit to 0.
- When the page is accessed, set **R** to 1
- Periodically set **R** to 0
- If the page is written to after loading from storage, set **M** to 0

The **NRU** System divides pages into 4 categories:
- **Category 0** pages that have not been written to or accessed recently
- **Category 1** pages that have not been referenced recently but have been modified recently
- **Category 2** pages that have been referenced recently but not modified recently
- **Category 3** pages that have been both referenced and modified recently

### Least Recently Used
The **LRU** algorithm uses past knowledge rather than predicting the future. It will replace the page that has not been used for the most amount of time. It associates **time of last use** with each page.

**LRU** works by either:
- Using a modified access bit to be a longer last access time stamp for each page
- Orders pages in a doubly linked list in order of access time

Linux uses a variant of LRU which works similarly to NRU but instead of choosing randomly selects the page at the end of a queue of global active pages.

![[image-16.png]]

### Clock
- Pages are organized in circular list with "hand" to point to the next replacement candidate
- The clock hand will tick through the pages. Each time it will check if that page's access bit is at 1 or 0. If it's 1, the clock will set it to 0 and move on. If it's 0, the page will be swapped out.

### Clock II
- Builds off the normal Clock algorithm, but introduce an additional hand. normal hand = V, new hand = C. C = V + D. If D is very small this algorithm is basically **Clock**. As D gets larger, this provides more time for cleared reference bits to be reset.

# Tuning the System
- Linux has a **Swappiness** bit with a 0 to 100 scale.
- The higher the value the greater the swapping frequency
- Higher swappiness can decrease performance by causing thrashing

# Thrashing
If a process does not have enough pages, the page default rate is very high.
- Page fault to get page
- Replace existing frame
- But quickly need replaced frame back
- This leads to: 
	- Low CPU utilization
	- Operating System thinking that it needs to increase the degree of multiprogramming
	- Another process added to the system

**THRASHING** : a process is busy swapping pages in and out

# Demand Paging
- Demand Paging is the Linux method of allocating physical memory to its processes
- This is a just-in-time or lazy method as processes are granted memory right before they require it
- Demand paging is handled by the Linux memory management system.
- The MMS is a record of all the virtual addresses which have not been allocated a physical memory space yet. Therefore, when a process tries to access one of theses ununitialized memory locations a page fault occurs and the system allocated the memory.

# Copy on Write
- Child and parent processes have distinct virtual memory spaces but both map to the same physical addresses.
- When a write operation occurs, the system dynamically allocates a fresh frame for the process
- **COW** makes use of the methods of page fault handling detailed previously
- The benefit of **COW** is that it increases efficiency since both the parent and child processes  can share pages up until a write operation is performed

# Out of memory killer
- the **Out of Memory Killer** is a worst case scenario -> insufficient memory => the kernel will run a killer process: **OOM: Out of Memory Killer**
- Terminate processes and free up resources
- The "Rules"
	- The Kernel requires enough memory to run its own processes
	- Try to free up a large amount of memory
	- Try and minimize the number of processes terminated
- Running processes are given a score and the highest is terminated
- 
 ![[image-17.png]]
