- **ASLR**: Address Space Layout Randomization
	- Memory management technique to improve System Security
- **NUMA**: Non-uniform memory access
	- Linux has multiple versions: interleaved and node-local
- **RDMA**: Remote Dynamic Memory Access
	- Enables transferring pages from other machines to local
- **NV-RAM**: Non-Volatile Memory
	- Byte-addressable, persistence data, immediate restart

# Techniques to impriove system security
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