A **PROGRAM** is a list of instructions
A **PROCESS** is a program in execution

**Code** get compiled into a system-readable **Program**, which, when executed, becomes a **Process**.

Programs are typically stored in disk memory whereas processes are stored in RAM.

#### Process States
- New
- Ready
- Running
- Waiting
- Terminated
![[Operating-Systems/attachments/image-1.png|573]]

#### Fork()
The fork call creates a child process from a parent process. they occupy the same virtual address space, with the only difference between them being the return value of the fork call. The fork call from the parent returns the Process Identifier of the child. If there is no child process fork() returns 0, and if a new process cannot be created it returns -1.

#### execve()
To create a child process from a *different* program than the one currently running, use the execve() call. The first parameter is the name of the executable file, the second parameter is the argument vector (argv in c programs), and the third parameter is a set of environment variables represented as key/value pairs.

#### exec()
The exec() call replaces the current process with a new one in the same context.

#### Wait() & exit()
The wait call pauses the execution of a parent process whilst it waits for the child process to call exit(). Exit finishes the execution of the child process, and if the parent process is waiting, resumes the parent process.

#### Metadata
Each process has metadata associated with it. This allows the OS to identify, execute, and manage every process.
Usually the relevant data is stored within a process control block (PCB)
The most basic metadata is a unique positive integer known as the Process ID (PID)
You can retrieve the PID using the getpid() call.

#### Threads
A thread is essentially a lightweight process. In Linux there is no distinction between processes and threads, everything is just an exectuable task.
Threads are smaller units within a process, and share that process' address space. They interact via shared data and context switching between threads is usually a  faster process than context switching between processes.
Threads are useful for pipelining.

#### Context Switching
A context switch is the action of replacing one executing process with another. This is what happens when you use the exec() call. During a context switch the context of the previous process is saved to the stack, and reloaded upon switching back.

![[Operating-Systems/attachments/image-2.png|389]]

#### Policies and Mechanisms
It is important to separate mechanisms and policies in design systems.
A **Mechanism** is the ability to do something.
A **Policy** is the logic that governs when the mechanism is and isn't used.

#### Signals
A signal is a way for the Kernel to communicate with a process.
Signals are uni-directional asynchronous notifications.
Signal processing is event-based.
Signals can be thought of as Software Interrupts

![[Operating-Systems/attachments/image-3.png|577]]


