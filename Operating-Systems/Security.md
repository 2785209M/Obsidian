[[Secure Communications]]

A **Capability** is an unforgable **Token Of Authority** that grants particular rights to perform an action in an **execution** context.
# Buffer Overflow
A buffer is an intermediary between a sender and a receiver. A place to store data during transfer until the receiver is ready. Buffer overflow occurs when you put more data in the buffer than it can hold. This causes the memory to overflow and overwrite the next physical memory slots beside the buffer.
Buffer overflow can cause DoS and cause a system to crash if it overwrites data that the system relies on to function.
If a hacker can intentionally cause a buffer overflow on a target machine and overwrite the **return address**, they can trick the target into executing malicious code they stored inside the overflow data, potentially giving them complete control of the system.
This can  be prevented by using memory safe languages like Python, Java, and C#, which have automatic memory management and eliminate this kind of issue. With C and C++ however developers must manually manage memory.

# Memory Tagging
Memory tagging utilizes the hardware, operating system, and memory allocator (such as malloc()).

**1. Tagging Memory (The Lock)**
The system divides memory into small blocks called granules which are usually 16 bytes. The hardware dedicates a small amount of memory (usually 4 bits) to act as a 'tag' for each granule.
**2. Tagging the Pointer (The Key)**
In 64-bit systems pointers are 64-bits wide, but since this is rarely all used, some of the bits (usually the top 8) are used to make a 'tag' that matches the memory granules.
**3. The Hardware Check**
Every time a memory operation is performed, the hardware performs a check, comparing the tag embedded in the pointer to the tag embedded in the granule that is being accessed (whether read from or written to). If the granule's tag doesn't match it is either corrupted or foreign (malicious) and the hardware triggers an exception that terminates the program before corruption occurs.

**Why this prevents buffer overflows:** The allocator in the buffer will have a specific tag - say 15. If it tries to overwrite data adjacent to the buffer it may have a tag of 2. This will immediately terminate the process and no data adjacent to the buffer will be overwritten.

**Why this prevents dangling buffer attacks:** If a hacker holds onto a dangling pointer (a pointer that still points to memory that has already been freed) and tries to use it to overwrite data, the operation will fail because the tag on the old pointer will be different to the tag on the new freed granule.

What is System Security?
Security is the blocking of unauthorized access and operation.
It serves to block people who want to steal, inject, or manipulate data, cause damage to a system, or disrupt services.

# Principles of Protection
**The principle of least privilege:** Only give programs the bare minimum amount of privileges to perform their tasks.

## Protection Rings
**Compartmentalization:** Separate data through use of permissions and access restrictions, or physically separate data using partitioning.
Rings are a representation of compartmentilisation. This includes rings for applications, kernel, and hypervisor
## Arm CPU Architecture
![[image-45.png]]
## Domains of Protection
Rings separate functions into domains and order them in a hierarchy. The system can be separated into hardware and software objects. The higher levels have access to the lower levels without having to perform special calls but it does not go both ways.
## Domain Implementation
**Domain switching in a file system**
Each File has a domain bit (setuid bit) associated with it. When the file is executed, the setuid bit is set to the user id of the user executing it. When execution completes user id is reset. This is technically a domain switch into and out of that specific user's domain.

**Domain switching via password**
the 'su' command allows you to switch to another user's domain by entering their name and password

**Domain switching via commands**
The 'sudo' command allows you to enter the local root user domain with a password 

## Access Matrix
A visual representation of the permissions specific domains have for specific objects.
![[image-46.png|414]]
Access matrices scale poorly and are hard to implement so access lists are usually used instead.
Access lists store permissions per object, listing only domains that have access.

# Role-based Access Control
Protections can be applied to non-file resources. 
Role-based access control is designed to implement least privilege.
Privilege is the right to execute a system call or use an option within a system call.
Privileges can be directly assigned to processes.
Users can be assigned roles which grant them privileges.

# Mandatory Access Control
Mandatory access control makes some files inaccessible to even the root user, making it more powerful than discretionary access control.

# SE Linux
Security enhanced Linux offers layers of policies on top of normal protection and has more rules on what can and can't be accessed.

# Capability-based systems
This is included in Linux, Android and others. They put root powers into coordinates on a bitmap represented by a bitmap bit. This gives finer control over privileged operations. Considered an improvement over root having all privileges.
![[image-47.png]]

# Sandboxing
Sandboxing is the practice of running a process in a limited environment. You impose a set of irremovable restrictions and the process is then unable to act on any resources beyond its allowed set.

# Be **Very** careful with string inputs
Always sanitize input data. Be careful with string lengths. This is how hackers get in.

# Code injection attack
A code injection attack can be performed when system code has a bug that allows executable code to be added or modified. The goal is typically a buffer overflow.