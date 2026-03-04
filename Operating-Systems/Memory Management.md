Operating Systems organize memory in a computing system

# Virtual Memory
Virtual memory is a representation of physical memory in a computer system that makes it seem like a large flat memory space rather than billions of little registers. Virtual memory requires supportive hardware such as a Memory Management Unit (MMU) . The main benefits of using virtual memory are process isolation, code relocation, and hardware abstraction.

## Process Isolation
Makes it impossible to modify another process' memory if unable to address that memory directly. Attempting to address invalid memory spaces may lead to segmentation faults. But this doesn't affect the entire system, only the program you're trying to run.
*Segmentation Fault:* an error raised by hardware when you attempt to access an area of restricted memory.

## Code relocation
Even if the physical location in memory changes, files can still be accessed using the same virtual address, making access straightforward for linking and loading tools.

## Hardware Abstraction
Virtual memory provides a uniform and hardware-independent view of memory, despite physical memory changing when more RAM is installed or the machine is modified.

## Memory Management Unit
 A Memory Management unit is a piece of Hardware that maps a virtual address to a physical address at run time. As such, the user program never really *sees* physical addresses, only logical addresses supplied by the MMU.

### Relocation Register
A relocation Register offsets the virtual address by a set value before sending it into main memory.

 ![[image-7.png|649]]
## Virtual Address Space Layout
If each memory location addresses a byte, then a 32-bit processor can address 2^32 bytes. This is equal to 4 Gigabytes of Address Space. 

![[image-8.png]]

## Pointers in C
A pointer in C is a variable that stores the memory address of another variable.

Pointers are useful because they give you direct access to your computers memory. This makes it easier to traverse arrays, takes less processing power to pass pointers into functions than large variables, and if you directly edit a pointer inside of a function then that function can effectively "return" multiple values.

Pointers allow you to utilize the stack frame, call **malloc()**, grow the heap using **brk()/sbrk()**, and map new regions into memory using **mmap()**.

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
### Page Table
Page tables avoid **External Fragmentation** as they allocate physical memory to processes whenever and wherever it is available. The physical memory does not need to be contiguous as the page table will map the physical memory frames to contiguous pages (at least they seem contiguous to the process).

Pages are given page numbers, which allow them to be mapped onto a page table, and a page offset, which when combined with their page number defines their space in physical memory.
![[image-12.png|416]]

The page table is really a piece of software that has its own memory space in RAM, but it's the MMU that actually reads the table to perform the translation between physical and virtual memory.

Generally small page sizes are accepted to be better as they minimize fragmentation. Smaller pages also increases the overhead for per-page metadata, which is why the middle ground of 4KB was settled on for Linux. Larger pages are at higher risk of internal fragmentation.