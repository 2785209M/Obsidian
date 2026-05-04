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


# Advanced Memory Topics
- **ASLR**: Address Space Layout Randomization
	- Memory management technique to improve System Security
- **NUMA**: Non-uniform memory access
	- Linux has multiple versions: interleaved and node-local
- **RDMA**: Remote Dynamic Memory Access
	- Enables transferring pages from other machines to local
- **NV-RAM**: Non-Volatile Memory
	- Byte-addressable, persistence data, immediate restart

# Techniques to improve system security
## No execute bit (NX bit)
- Indicates whether a memory page is executable
- Can prevent the stack and the heap from being executable
- Does not stop all buffer overflow attacks

## Address space layout randomization (ASLR)
- Randomizes the position of the stack, heap, and libraries in the process' VAS (Virtual address space)
- prevent buffer overflow attacks relying on knowledge of the location in executable memory

# Non-Uniform Memory Access
- NUMA is a computer memory architecture where the memory access times differ depending on the distance between the processor and the place where the memory is stored
- Processor caches can be used to reduce the variance in access latencies

## Optimising NUMA
- Interleaved memory allocation
	- It is allocated in a round-robin fashion across all the nodes
	- Ensures memory access times are uniform on average
- node-local
	- Assume the memory is likely to be accessed by threads running on cores in the same NUMA region
	- Allocates memory close to the processor which executes the malloc()
- *Distributed memory issue: Each processor has its own memory. The processor may need to communicate with remote processors if remote data is required*

# Non-CPU mediated memory access
## Direct memory access (DMA)
- Copies data between devices and memory buffers

## Remote dynamic memory access
- enables data to be transferred rapidly between nodes in LAN
- Copies memory from a remote buffer to a local buffer
- Used in cloud, e.g. VM migration and distributed storage services