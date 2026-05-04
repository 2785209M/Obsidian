## Paging
Linux uses a scheme called **paging** to manage resource allocation and memory management. Physical memory is divided into fixed size blocks called **Frames** and virtual memory is divided into fix sized blocks called **Pages**. The frames and pages in a system must be the same size (Usually 4KB) so the pages can be mapped to the frames. use the command **getconf PAGESIZE** in the terminal to view the page size on your own Linux machine.

### Fragmentation
#### External Fragmentation
External fragmentation is when memory is split up at the page level. It means there are gaps of unallocated pages between sections of filled pages, which may be too small to allocate to a process, causing wasted space.
![[image-10.png|308]]
#### Internal Fragmentation
Internal fragmentation is when the memory space inside of a page is divided poorly. There may be gaps between utilized memory that get wasted because they are too small to run any processes.
![[image-11.png|215]]

![[image-13.png|608]]

## Page Table
Page tables avoid **External Fragmentation** as they allocate physical memory to processes whenever and wherever it is available. The physical memory does not need to be contiguous as the page table will map the physical memory frames to contiguous pages (at least they seem contiguous to the process).

Pages are given page numbers, which allow them to be mapped onto a page table, and a page offset, which when combined with their page number defines their space in physical memory.
![[image-12.png|416]]

The page table is really a piece of software that has its own memory space in RAM, but it's the MMU that actually reads the table to perform the translation between physical and virtual memory.

Generally small page sizes are accepted to be better as they minimize fragmentation. Smaller pages also increases the overhead for per-page metadata, which is why the middle ground of 4KB was settled on for Linux. Larger pages are at higher risk of internal fragmentation.

The **Page Table** is stored in **Main Memory**. the **Page Table Base Register** points to the page table and the **Page table length register** Indicates the size of the page table. I this scheme every data/instruction access requires two memory accesses. One for the table and one for the data within the table. Two access memory problems can be solved using **Translation Look-aside Buffers**.

**Multi-Level Page Tables** are more space efficient than **Single Level Page Tables**.

## Page Table Structure
### Heriarchical Page Tables
Heirarchical paging reduces the amount of memory that a page table consumes. So do hashed page tables and inverted page tables. 
![[image-14.png]]
### Translation look-aside Buffer
The translation look-aside buffer can store a small number of page table mappings for faster lookup. This is known as caching. The cache must be flushed every so often to make space for new data which causes a context switch and can be resource expensive.

#### Steps in TLB Lookup:
1. CPU Generates Logical Address
2. CPU Checks to see if the address is in the TLB
3. If yes, returns the physical memory location of the data and doesn't check the page table
4. If no, performs time-consuming manual search of the page table and adds it to the TLB

### Memory Protection
Memory protection can be implemented in a physical memory by using a protection bit, which indicates whether the frame is read-only or read-write.

Memory protection can be implemented in a page table by using a valid/invalid bit. **Valid** indicates that the associated page **IS** within the processes address space, and is as such a legal page. An **Invalid** bit indicates that the page is **NOT** within the process' address space and is therefore an illegal bit.

Any violations of these protective bits results in a trap to the Kernel.
_(A trap is a Software Interrupt that triggers a switch from user mode to Kernel mode, allowing the OS to perform more privileged operations)_

### Page Tables in ARM
Multi Level hierarchical page tables usually have up to 5 levels:
- Page Global Directory (PGD) - One per process, base pointer in MMU
- Fourth Level Dictionary (P4D) - only in 5-level, not supported on ARM
- Page Upper Directory (PUD) - in 4 and 5-level, supported on AArch64
- Page Middle Directory (PMD) - Intermediate page table
- Page Table Entry (PTE) - leaf of page table, multiple pages to frame translation

Structures Vary based on Linux version and CPU Architecture. The default 32-bit raspberry pi is only 2-level.

### Page MetaData
Page table entries contain more data than just virtual -> physical address mapping. They also contain Metadata, such as memory protection, sharing, caching, etc. Each attribute is represented by individual bits.

**Lower Bits** -> Physical Address
**Higher Bits** -> Metadata

Attempting to read a page that doesn't have the correct permissions will result in a page fault.

On Linux, users can view memory data at the segment level in the directory /proc/PID/maps. The /proc/PID/pagemap directory contains actual page mapping and metadata.

### Arm Architectural Details
- System Control Coprocessor CP15
	- Translation based registers process specific page tables. 
- One-page based Registers#
	- Process Specific Address
	- OS Kernel Addresses
- Page Control Register Decides which base register is used for page table walks.
- OS Updates PGD at context switch to switch page tables.
- The cache flushes at context switch

# Multiple Page sizes
To design a page table with multiple page sizes, you need to use a hierarchical page table. Use the offset to choose the size of your base, lowest level page tables. an offset of 12 would give you 2^12 bytes, or 4KB. If we make each level thereafter span a specific number of bits (e.g. 8), the size of the page table would be multiplied by 2^8 each time you go up a level.
We have to set a Large Page (LP) bit inside each of the levels of the page table. The Memory management Unit will read this bit when it performs a table walk so it knows where to stop. If we set the LP flag to 1 in the third level the MMU will stop there and all bits thereafter will become the offset. In our example that would be 2^28 or 256MB.
To facilitate this structure we would either need to use multiple Transition Lookaside Buffers for each level or use a special TLB that is capable of processing multiple page sizes.
The OS would also need a memory allocator capable of reserving physical memory or the same size as the page table levels.

To determine the number of bits for the page number and the total number of pages, we need to break down the 20-bit address into two components: the **Page Number** and the **Page Offset**.
### 1. Calculate the Offset Bits
The page size determines how many bits are required for the offset (the address within a specific page).
- **Page Size** = 4 KB
- 4 KB=4×1024 bytes=4096 bytes
- In powers of 2: 4096=2^12
Since 2^12 bytes can be addressed within a single page, we need **12 bits** for the offset.
### 2. Calculate the Page Number Bits
The total address space is 20 bits. The address is split as follows:
Total Address Bits=Page Number Bits+Offset Bits
Substituting the known values:
20=Page Number Bits+12
Page Number Bits=20−12=8 bits
### 3. Calculate the Number of Pages
The number of pages in the system is determined by the number of possible values the page number bits can represent.
- **Number of Pages** = 2Page Number Bits
- Number of Pages=28=256 pages
**Summary:**
- **Bits for the page number:** 8 bits
- **Total number of pages:** 256 pages

# Common Metadata Bits and Their Purposes

### 1. The Present / Valid Bit
- **Purpose:** Indicates whether the page is currently residing in physical memory (RAM).
- **Usage:** If this bit is **0**, a "page fault" is triggered when the CPU tries to access it, signaling the OS to fetch the data from the disk. If it is **1**, the translation proceeds normally.    
### 2. Protection Bits (Read/Write/Execute)
- **Purpose:** Defines the permissions for the page.
- **Usage:** These bits prevent unauthorized actions, such as a program trying to write to a "read-only" code segment or executing data in the "stack" (a common security measure against buffer overflow attacks).
### 3. The Accessed (or Referenced) Bit
- **Purpose:** Tracks whether the page has been read or written to recently.
- **Usage:** This is critical for **Page Replacement Algorithms** (like LRU - Least Recently Used). When memory is full and the OS needs to kick a page out, it looks for pages where the Accessed bit is 0, as they haven't been used in a while.
### 4. The Dirty (or Modified) Bit
- **Purpose:** Indicates if the data in the page has been changed (written to) since it was loaded from the disk.
- **Usage:** If the OS needs to swap a page out of RAM, it checks this bit. If it's **1** (dirty), the OS must write the changes back to the disk. If it's **0** (clean), the OS can simply discard the page because the version on the disk is already identical.
### 5. The User/Supervisor (Privilege) Bit
- **Purpose:** Restricts access based on the CPU's current privilege level.
- **Usage:** This ensures that a regular user application cannot access memory belonging to the Operating System kernel, even if that memory is currently "Present."    
### 6. Caching Controls (PCD/PWT)
- **Purpose:** Controls how the page interacts with the hardware cache (L1/L2/L3).
- **Usage:** * **Page-level Cache Disable (PCD):** Prevents the data from being cached at all (essential for memory-mapped I/O devices where data changes outside of CPU control).
    - **Page-level Write-Through (PWT):** Forces writes to go immediately to main memory rather than staying in the cache.
## Why Is This Metadata Necessary?
Without these bits, the CPU would be "blind." It would know _where_ the memory is, but it wouldn't know:
- **If it's safe to touch** (Protection/Privilege). 
- **If it's actually there** (Present bit).
- **How to manage it under pressure** (Accessed/Dirty bits).