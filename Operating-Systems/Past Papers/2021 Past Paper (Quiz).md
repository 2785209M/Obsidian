# Memory
1. a
2. c
3. c x b
4. d 
5. / c
6. / a
7. / d
8. b x a
9. a 
10. c
11. / c
# Synchronization
12. b x d
13. b
14. / c
15. b x c
16. a

# Scheduling
17. a
18. a x d
19. d 
20. c
21. b
22. d

# 12/22 = 54.5%

# Notes
- **Multiprogramming** is the technique of switching the CPU to a differet task whilst its current task is waiting for an I/O operation. This increases CPU utilization.
- Having more main memory (RAM) allows more pages to be loaded into memory at once which reduces context switching which reduces the number of page faults and increases CPU efficiency.
- **worst case access time in a two-level page table with respect to physical memory**: Worst case scenario access time triples, because you need three addresses in total: one for the outer directory, one for the inner directory, and one for the actual data.
- You should know how to solve the producer-consumer problem with mutexes, semaphores, and barriers respectively
- A typical monitor pattern requires you to lock the mutex, wait for a condition variable in a while loop, manipulate data once the condition is met, signal other threads, and then unlock the mutex.