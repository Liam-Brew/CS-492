# Process Interactions

## Table of Contents

- [Process Interactions](#process-interactions)
  - [Table of Contents](#table-of-contents)
  - [Process Competition](#process-competition)
  - [Critical Section Problem](#critical-section-problem)
  - [Classic Synchronization Problems](#classic-synchronization-problems)
    - [Readers-Writers Problem](#readers-writers-problem)
    - [Dining-Philosophers Problem](#dining-philosophers-problem)
    - [Elevator Algorithm](#elevator-algorithm)

## Process Competition

**Concurrency** is the act of multiple processes or threads executing at the same time. When multiple physical CPUs are available, the process may execute in parallel. On a single CPU concurrency may be achieved by time-sharing

## Critical Section Problem

A **critical section** is a segment of code that cannot be entered by a process while another process is executing a corresponding segment of the code

![critical_section](/notes/assets/concurrency/critical_section.PNG)

Solutions to this problem must satisfy the following requirements:

1. **mutual exclusion**: guarantee that only one process may be executing within the critical section
2. **lockout**: prevent processes not attempting to enter the critical section from preventing other processes from entering
3. **starvation**: prevent a process or a group of processes from repeatedly entering the CS while other processes are waiting to enter
4. **deadlock**: prevent multiple processes from entering the critical section at the same time from indefinitely blocking each other

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