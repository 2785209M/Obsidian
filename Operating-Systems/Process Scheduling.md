**Scheduling** is the action of selecting which processes to run and when.
A **Scheduling mechanism** enables different processes to execute on the CPU. Threads and processes are treated as the same thing in linux.
A **scheduling policy** determines which processes should run and for how long.

#### Process Behaviour
- CPU Bursts: Time the process using CPU clock cycles
- I/O Bursts: Time the process by waiting for a resource. E.g User input or file data.
CPU Bursts are generally faster.

#### Scheduling Principles
- Pre-emptive: CPU timer interrupt can preempt long running processes (Linux does this)
- Non-preemptive: Processes voluntarily yield CPU
- Scheduling Policy: The strategy for determining which task should be run next from a selection of executable tasks
- 
![[image-6.png]]


#### Scheduling Criteria
- CPU utilization: the CPU should ideally be utilized 100% of the time
- Throughput: The number of processes completed per unit time
- Load average: the average number of processes in the ready queue
- Turnaround time: The time it takes for a particular task to complete
- Waiting time: The amount of time a particular task spends in the ready queue
- Response Time: The time take  between submitting a request and receiving a response

#### Scheduling Policies
- FCFS
- RR
- SJF
- Highest Priority
- Earliest Deadling

#### Real time Systems
- Soft real time: offers a no-gaurantee best effort approach. Missed deadlines are undesirable but not disastrous
- Hard real time: Missing a deadline is absolutely catastrophic and cannot happen.

Types of soft realtime systems: FCFS, RR
Hard realtime scheduling: EDF, HPF, SJF

Linux mostly uses soft real time scheduling.

#### Linux Task Priorities
Linux can assign values to tasks from 0 to 139.
100-139 is low priority without real time requirement
0-100 always have priority and use FCFS or RR scheduling
In Linux all scheduling is preemptive and all threads can be interrupted

The Linux kernel schedules **THREADS** not processes. (But they're treated as the same thing?)

![[image-4.png|647]]

Linux has a scheduler called the **Completely Fair Scheduler** (CFS). CFS attempts to balance virtual runtime over all tasks.

![[image-5.png]]


#### Linux Scheduling Commands
- For a process to be executed: nice -n 12 command
- For a currently running process: renice -n 15 -p pid
- for real time processes
	- chrt to retreive/set scheduling attributes
	- sudo chrt --rr 32 pwd 

Soft real time schedules have a higher priority than the non-real time threads. Real time task queues are kept in a linked list and a bitmap indicates which queues are empty. Policies: Sched_FIFO and SCHED_RR.

Hard real time tasks use the SCHED_deadline policy.

#### Kernel Preemption models
- No forced preemption (server)
- voluntary kernel premption (desktop)
- preemptible kernel (low latency desktop)
- preemptible kernel (basic real time)
- full preemptible kernel (real time)