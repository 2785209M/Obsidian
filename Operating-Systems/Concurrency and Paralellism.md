**Concurrency**: When more than one task is running at the same time on a system. This is a property of the workload and not the system.
**Parallelism**: Hardware support for multiple parallel instructions. Multiple CPU cores allowing for the execution of multiple tasks in Parallel. This is a property of the System and not the workload.

If the Kernel supports parallelism it will try to speed up the execution of tasks using **Pipelining**.

- In a concurrent Program, several threads are running at the saem time as the user expects several actions to be happening at the same time.
-  In a Parallel program, the work that would be performed by a single CPU is split up and handed to multiple CPUs who execute each part in parallel.
- Instruction-Level Parallelism: A processor capability that allows the simultaneous execution of multiple instructions from a single program at the same time.

If you ran a parallel program on a single core system, it would still run, it would just be slower.

Many of the problems encountered in parallel computing are also encountered in parallel programming.

# Producer/Consumer Problem
Between a producer and a consumer, there is typically a Buffer. This buffer is bounded, is a shared resource, and writes to the buffer must leave the buffer in a consistent state. To accomplish this the buffer should be protected by **Access Control**.

![[image-19.png]]


# Race Conditions
A race condition is a problem that can occur when multiple processes share the same memory. They happen when two or more processes are trying to alter the same data. The sequence in which they end up changing the data affects the result. 

- Sequential Execution is when processes execute one after another within the race condition.

- Interleaved Execution is when the execution of two processes are jumbled up and interwoven with each other.
# Critical Region
A **Critical Region** is an area of code accessing a common resource that only allows one process/thread to access it at a time. 

## Protecting a Critical Region
- **Serialized Access** ensures that the critical region can only be accessed by one thread/process at any given time. it is provided by the OS.
**There are multiple methods of ensuring serialized access**
- **Implementing a Lock**: before a process accesses the critical region it must acquire the lock. once it has acquired the lock, no other processes can access the lock until this process releases it.
For example, if the processes are being interleaved during a race condition but process 1 starts first, it will begin by requesting the lock. This will give it the lock. Then Process 2 will ask for the lock but won't get it and therefore won't begin. Process 2 will only begin once process 1 has finished and released the lock.

# Blocking vs Non-Blocking
A **blocking region** will send a message and stop execution whilst it waits for a response. On the other end, the **Reciever** may be waiting until it receives a message.

A **Non-Blocking Region** will Send/Recieve messages and then continue execution.

# Deadlock
A **Deadlock** is a situation in which 2 processes are each holding a piece of data the other needs in order to finish, causing the process to freeze by creating a cycle of dependency that prevents further execution.

For example, P1 wants both resources and has locked one of them, but P2 also wants both resources and has locked the other. Neither will give up the resource they hold so each will be stuck in a cycle of perpetually requesting the resource from the other.

# Characteristics of Deadlock

- **Mutual Exclusion**: Only one thread at a time being able to access a resource
- **Hold and Wait**: A thread holding a resource is waiting to acquire additional resources held by other threads
- **No Preemption**: A resource can be released only voluntarily by the thread holding it after that thread has completed its task
- **Circular Wait**: multiple threads will be holding data that is needed by another, whilst that thread holds data that it also needs, so neither of them can finish.

# Solution to Deadlock
- Modern Operating Systems try to prevent Deadlocks using algorithms, such as Bankers Algorithm. If this fails, the processes causing the deadlock can be terminated, or the program may be left hanging.


# Synchronization primatives
- User-space primitives include **mutexes** and **semaphores**.

- kernel primitives include **Atomics** (Read-modify-write operations), **memory barriers**, **Spin locks**,  and **futexes**.

## mutex
a mutex is a boolean variable with 2 states - locked and unlocked. if a mutex is locked it prevemts any other threads from accessing its resource.

# semaphore
a semaphore is a counter that is atomically incremented and decremented.
• If (--sem & sem <1) block
• Block until sem >= 1

# Atomics
atomics include RMW operations and two classes, atomic_t and atomic64.t.
atomics are special because they cannot be interrupted. They either succeed entirely or fail completely. To the system they are seen as a single, uninterruptible action.
# Barriers
A barrier is a synchronization mechanism that forces threads/processes to stop at a specific point in the program and wait until all other participating threads/processes are at the same point before proceeding.
Memory barriers compensate for varying memory models is different processors, which can be either strict or relaxed. Memory barriers make it so all memory modifications will be visible to all threads on all processors no matter what.

- **General Barrier**: servers as an instruction to the compiler to prevent reordering of memory access
- **Mandatory Barrier**: Write/Read
- **SMP Conditional Barrier**: Symmetric multiprocessing barrier, ensures data consistency between multiple cores
- **Implicit Barrier**: Locking constructs to act as implicit SMP barriers

# Locks
**Spin Locks** are the simplest form of locking. They are a while loop. while(!has_lock){trytogetlock}. The problem with this is that it occupies the CPU.

# Futexes
A **Futex** is a fast user space mutex. They are Linux-Specific and optimized for performance in the case that there is no contention for resources.

Futexes occur entirely in user space for non-contended cases. The kernel handles contended cases.

Futexes are lightweight and scalable, and use Semaphore semantics.

# Types of Parallelism
- **Data Parallelism**: distributes subsets of the same data across cores, which perform the same operation.
- **Task Parallelism**: Distributes threads across cores, which each perform a unique operation 

**GPUs** are designed specifically for **Data Parallelism**, NOT task parallelism. They support instruction-level parallelism, but are not designed for concurrent tasks.

# Amdahl's Law
*'The overall performance improvement gained from the optimization of a single part of a system is limited by the fraction of time the optimized part is actually used'* 

![[image-20.png]]

Adding more cores speeds up parallel code, but does almost nothing to serial code.


# POSIX Threads

**POSIX** is an API that provides data types and API calls to manage threads.
POSIX threads are commonly known as **pthreadsz**.

A single process can contain multiple threads, all of which execute the same program. 

Threads share the same global memory but each have their own stack.

IF the OS does not provide kernel threads, the pthread API will provide user-level threads.

To use pthread, you have to include it in your script by using "#include <pthread.h>", and specify -lpthread in the command line.

The main pthread management calls are:
**pthread_create()**
**pthread_exit()**
**pthread_join()**

The first thread created by the operating system when it creates the process. It is this thread that executes **main()**.
After processing arguments, **main()** then creates any other threads required by the application.

When the routine invoked upon thread creation returns, the thread is killed.

A routine in a thread should **NEVER** invoke exit(), as this will cause the process to terminate, killing all threads in the process.

A thread can pause execution until another thread has finished by using pthread_join().

# pthread_create()
• pthread_t *thread_id
this argument is the address of a variable into which the thread ID of the created thread will be
written
• pthread_attr_t *attributes
this argument allows you to fine tune various parameters;
to use the defaults pass NULL
• void *(*thread_function)(void *)
this argument is the function to execute in the new thread; the thread will terminate when this
function returns, or if that thread invokes pthread_exit()
• void *arguments
this argument is passed as the single parameter to the function;
to pass no arguments to the function, specify NULL
• On success, the identifier of the newly created thread is stored in the location pointed by the
thread_id argument, and a 0 is returned. On error, a non-zero value is returned.
• The newly created thread is ready to be scheduled immediately; in particular, it may start running
before the parent thread executes the next statement after the call to pthread_create()

# pthread_exit()
• void *retval
the return value of the thread; another thread can obtain this return
value by invoking pthread_join()
• this function never returns – the thread that was executing the call to
pthread_exit() has committed suicide

# pthread_join()
• pthread_t thread
this argument is the id of the thread whose termination is of interest
• void **thread_return
If thread_return is not NULL, the return value of thread is stored in
the location pointed to by thread_return
• On success, the return value of thread is stored in the location
pointed to by thread_return, and 0 is the function return. On error,
a non-zero value is returned as the value of the function.

# OpenMP

**OpenMP** is an implementation of multithreading whereby a master thread forks into a specific number of slave threads.
The threads then execute concurrently, with the runtime environment allocating different threads to different processors.

# Control Directives
• # pragma omp critical
• Critical section, executed by one thread at a
time
• acquiring a lock, carries a substantial
overhead.
• multiple critical sections, are mutually
exclusive
• # pragma omp atomic
• Section of code atomically updated, some
performance advantages over critical section
• limited to the update of a single memory
location, not exclusive, more efficient, assume
hardware mechanism for making them critical.
• # pragma omp barrier
• all active threads will stop until all threads
have arrived at that point. Guarantees those
calculation finish.

# MPI

Message Passing Interface is an interface designed for high performance computing in which tasks manage their own personal memory sections instead of sharing. This interface relies on messages passed between tasks where communication is desired

Communicators represent rules that define which processes can communicate with what
Tags are used to match specific senders to specific receivers

Can run on almost any system
The API is complex so has significant overhead in design and programming

# OpenCL

**OpenCL** is an open-source standard for parallel computing
It is designed for portability across platforms (e.g. GPUs and multicore processors)

It consists of a host connected to multiple compute devices
each device consists of multiple compute units
Each is made up of multiple processing elements
All compute units can access a global constant memory as well as their own private memory

This was Superceded by SYCL ()