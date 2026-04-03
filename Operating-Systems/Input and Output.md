I/O is managed through ports, busses, and device controllers which connect to various devices.

**Device Drivers** encapsulate device details, and present a uniform device-access interface to the I/O subsystem.

In a **Linux** system, you can use **lsblk or lsusb** in the command prompt to view devices in/connected to your device.
On a Mac you can use ioreg or System Information.

## I/O Hardware Concepts
There is a large variety of I/O devices, including **Storage, Transmission, and Human-Interface**.

- **Port** - Connection point for a device
- **Bus** - Daisy Chain or shared direct access
	- **PCI** - a bus that's common in PCs and servers
	- **Expansion Bus** - connects relatively slow devices
	- **Serial Attached SCSI (SAS)** - common disk interface
- **Controller (host adapter)** - electronics that operate port, bus, or device
	- Sometimes integrated, sometimes separate circuit
	- Contains processor, microcode, private memory, bus controller, etc.

- **Fiber Channel** - complex controller, usually in a separate circuit
- I/O instructions control devices

- Devices has addresses, used by:
	- Direct I/O instructions
	- **Memory-mapped I/O**
		- Device data and command registers mapped to processor address space
		- Especially for large address spaces

![[image-21.png]]

![[image-22.png]]

![[image-23.png]]

**I/O is generally digital, but sometimes analog** 
**I/O bandwidth is typically measured in bps (bits per second)**

![[image-24.png]]

# Linux Device Abstractions
There are **Three** kinds of devices in Linux

- **Character** devices
	- Stream of bytes, file-based abstraction (read/write), not buffered
- **Block** devices
	- Fixed size chunks (blocks) of data, storage based abstraction, subject to buffering
- **Net** devices
	- Operate on packets

## Application I/O interfaces
- I/O system calls encapsulate device behaviors in generic classes
- Device-driver layer hides differences among I/O controllers from the kernel
- New devices talking already-implemented protocols need no extra work
- Each OS has its own I/O subsystem structures and device driver frameworks
- Devices vary in many dimensions:
	- character stream or block
	- sequential or random-access
	- synchronous or asynchronous
	- sharable or dedicated
	- speed of operation
	- read-write, read only, or write only

![[image-25.png]]

# Device Management Strategies
- **Polling**
- **Interrupts**
- **Direct memory access**
## Polling
For each byte of I/O:
1. Read busy bit from status register until 0
2. Host sets read or write bit and if write copies data into data-out register
3. host sets command-ready bit
4. Controller sets busy bit, executes transfer
5. Controller clears busy bit, error bit, command-ready bit when transfer done

Step 1 is a **busy-wait** cycle to wait for I/O from device, which is reasonable if the device is fast, but not inefficient if the device is slow.

![[image-26.png]]


## Interrupts
- Polling can happen in 3 instruction cycles:
	- **Read**, **logical-and** to extract status bit, **branch** if not zero

- CPU **interrupt-request line** triggered by I/O device
	- Checked by processor after each instruction
- **Interrupt handler** receives interrupts
	- **Maskable** to ignore or delay some interrupts
- **Interrupt vector** to dispatch interrupt to collect handler
	- context switch at start and end
	- Based on priority
	- some **nonmaskable**
	- Interrupt chaining if more than one device at same time interrupt number

- The interrupt mechanism is also used for **exceptions**
	- Terminate process, crash system due to hardware error
- Page fault executes when memory access error
- System call executes via **Trap** to trigger kernel to execute request
- Multi-CPU systems can process interrupts concurrently
	- If operating system designed to handle it
- Used for time-sensitive processing, frequent, must be fast

# Interrupt Handling in Linux
An interrupt handler is registered with the **request_irq()** function
Required parameters include the interrupt number, handler function, and the associated device name.
While the system is servicing one interrupt, another interrupt may arrive concurrently. Some interrupt handlers may be interrupted, i.e., they are re-entrant. Others may not be interrupted.

## Why is I/O slow?
I/O is slow due to the constant need for context switches and frequently copying data from kernel memory to user memory.
**Buffering** can be used to improve performance. The objective is to batch small units of data into a larger unit and process this in bulk. Buffering quantizes data processing.
A buffer is essentially a temporary storage location for data being transferred from one place to another.

# Device Drivers
From a user's perspective, everything is a file. The **Device_file_read** function is doing the data transfer. Devices are registered/unregistered in the Kernel via **register_chrdev**. Every device receives a numeric ID.

## Blocking vs Non-Blocking I/O
IO commands from user space may return immediately, or they might wait until the operation completes when all data has been transferred (blocking).

**Blocking I/O**:
- Need to wait for actions to complete
- Slower compared to clock speed
- The thread that initiated the I/O is unable to do useful work while waiting
**Non-blocking I/O**:
- Returns immediately, performing as much data transfer as currently possible with the specified device
- If no data transfer can be performed, an error status code is returned
**Asynchronous I/O**:
- Process runs whilst I/O executes
- Difficult to use
- I/O subsystem signals process when I/O completed

In Unix, which uses file descriptor flags, the O_NONBLOCK flag indicates that an open file should support non-blocking calls.

![[image-27.png]]


# Direct Memory Access
- Used to avoid **Programmed I/O** for large data movement
- Requires **DMA** controller
- Bypasses CPU to transfer data direclty between I/O device and memory
- OS writes DMA command block into memory

![[image-28.png]]


# Key takeaways
- Key Linux concept: everything is a file
- I/O devices might be character or block based
- OS interacts with devices via a **Driver**
- Driver might handle comms with device via
1. polling
2. interrupt handling
3. direct memory access