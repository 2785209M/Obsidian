# Aims
The aim of this exercise is to develop a piece of code that will function as a **driver** for a simple disk device. 

The disk scheduling model that should be used is **FCFS**. The code will be called to pass packets of data to/from the disk. These packets will be passed from the disks to appropriate waiting callers.

**This program will be implemented in C using the pthread library**

The core of this solution will be the implementation of one or more threads which move packets between queues/buffers/(other things). 

The pthread library is used to create and manage threads, and provides support for both mutual exclusion locks and condition variables (which support a simple wait/signal mechanism).

# The Problem
We will be developing an abstract data type called DiskDriver, and calls will be made to an instance of the DiskDriver ADT. Calls will be made to the DiskDriver from the application level.

The code will pass around **SectorDescriptors**. SectorDescriptors are an ADT. We don't need to know anything about them. There will be a **SectorDescriptor Constructor**, which will be handed a region of memory (a **void** pointer and a length) and it divides that region of memory into pieces, each piece being the right size to hold a SectorDescriptor.

The SectorDescriptors are then passed to a component called the **FreeSectorDescriptorStore**. This is an unbounded container which will be used by the code and the test harness as a bank from which to acquire **SectorDescriptors**. Once they have been used, the SectorDescriptors must be passed back to the FreeSectorDescriptorStore.

We may use existing code from the test harness for the **FreeSectorDecriptorStore**. We may also use the **BoundedBuffer** provided on Moodle if we need it. *All other code must be our own work*.

The test harness includes fake applications that will require **SectoreDescriptors** from the store, initialize their contents and then pass them to the code. The fake application makes **two** kinds of requests: **1) Queue the write of a sector to disk, or 2) queue up a read for a sector from the disk**. After making each kind of request, the application must make a subsequent call to either **1) determine that the write was successful, or 2) retrieve the sector after it has been read**.

The harness also includes a fake **DiskDevice** to which you can pass **SectorDescriptors** for dispatch. The complication is that the DiskDevice can only write one sector at a time, and is quite slow in doing so. Therefore, you may have to queue up other requests. An application thread does not have to wait for its sector to be sent to the disk. Once it has handed the sector to your code, it is allowed to engage in other activity, returning to your driver to determine that the write has been successful when convenient for the application. This means you are likely to need dedicated threads within your code to pass sectors to the disk device. 

Once a sector has been sent to the disk, the code **must** pass the descriptor back to the **FreeDescriptorStore**.

The **DiskDevice** can only read one sector at a time from the disk. This means that you should always have a read outstanding, if one is available, on the DiskDevice. To maximize the throughput of the DiskDevice, the processing of recently read sectors must be minimal, handing it over to another (possibly buffered) part of your code for retrieval by the application before initiating another read. The application does not wait for sector retrieval, but it will eventually return to the driver to check if the read has been successful and to access the data from the read disk.

Sectors that have been read will be passed to the fake application layer, and eventually all sector descriptors will be returned to the free sector descriptor store by the applications.

This code is supposed to live inside the Operating System. This means you need to use bounded data structures in the parts of your code that are involved in handling the passing of SectorDescriptors between the applications and the disk device (this means you cannot use malloc()). There are several factors which could cause the application threads to block: They are awaiting incoming sectors, the buffers for incoming/outgoing sectors are full, or because there are no free Descriptors available.

In summary, you have **multiple applications** that run at their own pace (most likely using threads), and a **disk device** for which the requests must be serialized. Your **disk driver** has to bridge between the two despite their mismatching processing rates. You must use pthreads and bounded data structures to provide this bridging function to implement the spec in diskdriver.h.

It is recommended to begin with a sketch for your design, and develop an outline for the rough code needed to do it. The sketch should look like an interaction/collaboration diagram. This diagram is part of the submission. 