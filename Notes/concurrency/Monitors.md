# Monitors

## Table of Contents

- [Monitors](#monitors)
  - [Table of Contents](#table-of-contents)
  - [Basic Principles of Monitors](#basic-principles-of-monitors)
  - [Monitors and Bounded-Buffer](#monitors-and-bounded-buffer)
  - [Monitors with Priority Waits](#monitors-with-priority-waits)
  - [Monitor Implementation](#monitor-implementation)

## Basic Principles of Monitors

A **monitor** is a synchronization primitive implemented using P and V operations. Monitors can contain any number of internal functions

A **condition variable** is a named queue on which processes can wait for some condition to become true. They are not traditional variables and are instead the names of queues. They are accesses via two special operations:

- **c.wait**: causes the executing process to block and be placed on a waiting queue associated with the condition variable c
- **c.signal**: reactivates the process at the head of the queue associated with the condition variable c

A monitor's implementation must:

- guarantee that the functions are mutually exclusive and that only one process at a time may be executing within the monitor
- provide condition variables such that a process can step outside the monitor while waiting for a condition, thereby not preventing other processes from entering the monitor

## Monitors and Bounded-Buffer

Monitors can solve the bounded-buffer problem as they guarantee mutual exclusion. This means that the data deposit and removal functions automatically become critical sections. This solution works for multiple producers and multiple consumers

A counter, full_slots, is used to dictate when the consumer and producer may proceed:

- full_slots < n: producer may proceed
- full_slots > 0: consumer may proceed

## Monitors with Priority Waits

A **priority wait** has the form c.wait(p), wherein c is a conditional variable and p is an integer specifying a priority to which processes blocked on c are reactivated

![alarm_clock_monitor](/notes/assets/concurrency/alarm_clock_monitor.PNG)

## Monitor Implementation

A compiler uses P and V operations on semaphores to enforce the features of a monitor:

- a semaphore mutex (initialized to 1) is used to enforce mutual exclusion among the functions
- a semaphore c (initialized to 0) is used for each condition variable c to allow a process to block itself on the condition
- a semaphore urg (initialized to 0) is used to block a process executing a signal operation
- associated with each semaphore c is a counter c_nt, which tracks the number of processes blocked on c
- associated with the semaphore urg is a counter urg_cnt, which keeps track of the number of processes blocked on urg

**c.wait**:
```
c_cnt = c_cnt + 1
if (urg_cnt > 0) V(urg)
    else V(mutex)
P(c)
c_cnt = c_cnt - 1.
```

**c.signal**:
```
if (c_cnt > 0)
    urg_cnt = urg_cnt + 1
    V(c)
    P(urg)
    urg_cnt = urg_cnt - 1
```