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