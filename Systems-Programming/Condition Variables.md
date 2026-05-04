A condition variable is a struct implemented in the pthread library that helps manage threads. It allows threads to enter a waiting state when they are not doing work to free CPU cycles.
Condition variables are always used in partnership with a mutex.
## Functions
- `pthread_cond_init()` - used to initialize the condition variable. Must be called before use.
- `pthread_cond_wait()` - Puts the thread that calls it to sleep and unlocks the mutex atomically (at the exact same time)
- `pthread_cond_signal()` - wakes up a sleeping thread from the condition variable's queue
- `pthread_cond_broadcast()` -  wakes up all threads that are currently blocked using the condition variable. Typically used to wake up all threads to for shutdown.
- `pthread_cond_destroy()` - Destroys the condition variable and frees any internal system resources associated with it

