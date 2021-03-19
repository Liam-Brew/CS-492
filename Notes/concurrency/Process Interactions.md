# Process Interactions

## Table of Contents

- [Process Interactions](#process-interactions)
  - [Table of Contents](#table-of-contents)
  - [Process Competition](#process-competition)
  - [Critical Section Problem](#critical-section-problem)
    - [Lock Variables](#lock-variables)
    - [Strict Alternation](#strict-alternation)
    - [Peterson's Solution](#petersons-solution)
  - [Test-and-Set Lock (TSL) Instruction](#test-and-set-lock-tsl-instruction)
  - [Classic Synchronization Problems](#classic-synchronization-problems)
    - [Readers-Writers Problem](#readers-writers-problem)
    - [Dining-Philosophers Problem](#dining-philosophers-problem)
    - [Elevator Algorithm](#elevator-algorithm)

## Process Competition

**Concurrency** is the act of multiple processes or threads executing at the same time. When multiple physical CPUs are available, the process may execute in parallel. On a single CPU concurrency may be achieved by time-sharing

A **race condition** is when two or more processes are reading or writing shared data, with the final result depending on who ran when

## Critical Section Problem

A **critical section** is a segment of code that cannot be entered by a process while another process is executing a corresponding segment of the code

![critical_section](/notes/assets/concurrency/critical_section.PNG)

Solutions to this problem must satisfy the following requirements:

1. **mutual exclusion**: guarantee that only one process may be executing within the critical section
2. **lockout**: prevent processes not attempting to enter the critical section from preventing other processes from entering
3. **starvation**: prevent a process or a group of processes from repeatedly entering the CS while other processes are waiting to enter
4. **deadlock**: prevent multiple processes from entering the critical section at the same time from indefinitely blocking each other

To enter a critical section interrupts need to be disabled. Upon leaving the critical section interrupts need to be re-enabled. This is a privileged operation and can therefore not be called by user space

### Lock Variables

In addition to interrupts, **lock variables** can also be used. The idea of these is to use a shared variable (lock) between processes:

- 0 means no process is in its critical region. 0 is the initial value
- 1 means that some process is in its critical region

```c
// Process A
while(TRUE){
  while (lock != 0); // Loop
  lock = 1;
  critical_region(); // Work
  lock = 0; 
  noncritical_region();
}

// Process B
while(TRUE){
  while (lock != 0); // Loop
  lock = 1; 
  critical_region(); // Work
  lock = 0;
  noncritical_region();
}
```

The problem with this approach is that the program continuously checks to see the state of the lock. This wastes resources

### Strict Alternation

**Strict alternation** solves this problem. Similar to locks, the idea of this is to have a single shared variable (**turn**) between processes. This causes processes that run different code to strictly alternate. The value of the turn is based on the following:

- 0 means that its process 0's turn
- 1 means its process 1's turn

The turn is initially set with 0 or 1 based on who should start first. This allows both processes to check different conditions instead of the same as with locks

```c
// Process A
while(TRUE){
  while (turn != 0); // Loop
  critical_region(); 
  turn = 1; 
  noncritical_region();
}

// Process B
while(TRUE){
  while (turn != 1); // Loop
  critical_region(); // Work
  turn = 0;
  noncritical_region();
}
```

The advantages of strict alternation is that race conditions are avoided. The disadvantage is that this violates condition 3 of critical sections

### Peterson's Solution

The idea of this is to create two shared variables (**turn**, **flags[]**) between processes:

- **int turn** imposes an access order (in this case FIFO)
- **bool flags[]** is an array that specifies for each process its interest to enter the critical section
  - initially, **flags[2]={FALSE,FALSE}**

```c
// Process A
while(TRUE){
  flags[0] = TRUE;
  turn = 0;
  while (turn == 0 && flags[1] == TRUE);
  critical_region(); // Work
  flags[0] = FALSE;
  noncritical_region();
}

// Process B
while(TRUE){
  flags[1] = TRUE;
  turn = 1;
  while (turn == 1 && flags[0] == TRUE);
  critical_region(); // Work
  flags[1] = FALSE;
  noncritical_region();
}
```

This is not used often as it is difficult to scale to multiple processes

## Test-and-Set Lock (TSL) Instruction

One single machine instruction: *TSL register*, *memory_address*

This **reads and modifies** the content of a memory word atomically:

- returns in *register* the current value of the memory word at *memory_address*
- sets the value of the memory word at *memory_address* to TRUE (usually non-zero value)
- operations cannot be interrupted during execution

The idea of this is to have a single shared variable (**lock** in memory at **LOCK_ADDR**) between processes. This uses TSL instead of while/assignment:

```c
// Process A
while(TRUE){
  enter_region:
    TSL REGISTER,LOCK_ADDR
    CMP REGISTER,#0
    JNE enter_region
  critical_region(); // Work
  leave region:
    MOV LOCK_ADDR,#0
  noncritical_region();
}

// Process B
while(TRUE){
  enter_region:
    TSL REGISTER,LOCK_ADDR
    CMP REGISTER,#0
    JNE enter_region
  critical_region(); // Work
  leave region:
    MOV LOCK_ADDR,#0
  noncritical_region();
}
```

The advantages of this solution are:

- no races
- strict alternation free
- works for many processes as-is
- simple

The disadvantages are:

- scalability on multi-core setups
- requires hardware support

## Classic Synchronization Problems

### Readers-Writers Problem

This is an extension of the critical section problem where two types of processes, readers and writers, compete for access to a common resource. Readers are allowed to enter the critical section concurrently but only one writer is allowed at a time

The challenge is to guarantee maximum concurrency of readers while preventing the starvation of either type of process. Two rules must be enforced:

- a reader is permitted to join other readers currently in the critical section only when no writer is waiting. When the last reader exits the critical section, the writer is allowed to enter (guarantees writers cannot starve)
- all readers that have arrived while a writer is in the critical section must be allowed to enter before the next writer (guarantees readers cannot starve)

A **monitor** can provide a solution to this using four functions:

- **start_read**: called by a reader to get permission to read
- **end_read**: called when finished reading
- **start_write**: called by a writer to get permission to write
- **end_write**: called by a writer when finished writing

Two counters, reading, and writing, are used to track the number of readers and writers currently in the critical section

Two condition variables, ok_to_read and ok_to_write, are used to block the readers and writers

A count(c) primitive returns the number of processes blocked (e.g., count(ok_to_read) calls the number of processes blocked on ok_to_read)

![monitor_readers_writers](/notes/assets/concurrency/monitor_readers_writers.PNG)

### Dining-Philosophers Problem

This represents situations where multiple processes compete for shared resources but each process needs more than one resource at a time. The challenge is to prevent deadlock while guaranteeing that any two nonadjacent processes can always operate concurrently using resources shared with each process's adjacent process

This is known as deadlock and can be caused by any number of processes more than 1

There are several approaches to avoid the problem:

- request both resources at the same time in a critical section
- one process requests resources in an opposite order than all the other processes

A **monitor** can be used to solve this problem. Each process can be in one of three states: idle, requesting resources, or using resources

The pickup(i) function switches p[i] from idling to using resources if p[i-1] and p[i+1] are not using resources. Deadlock is not possible because no resources are used at the same time, with the act of picking up the resources being represented implicitly by the single state transition from idling to using resources

The putdown(i) function guarantees concurrent execution by enabling p[i-1] if p[i-2] is not eating and similarly enables the neighbor p[i+1] if p[i+2] is not eating

The condition (state[i] == requesting_resources) is always true when p[i] calls the pickup() function on itself: test(i)

The condition (state[i - 1 mod 5] != using_resources) is always true when p[i] calls putdown() on the right neighbor: test(i + 1 mod 5)

![monitor_dining_philosophers](/notes/assets/concurrency/monitor_dining_philosophers.PNG)

### Elevator Algorithm

A storage disk consists of n concentric tracks that need to be accessed by read/write requests in an unspecific order. The goal is to minimize the travel distance between tracks while preventing starvation

The elevator (representing the read-write head of the disk) maintains a motion, up or down, so long as disk tracks exist in the current direction

When moving up and no more requests for higher tracks exist, the elevator reverses direction and services all requests in descending order. When the request for the lowest floor is serviced, the elevator again reverses direction and moves up
