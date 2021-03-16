# Semaphores

## Table of Contents

- [Semaphores](#semaphores)
  - [Table of Contents](#table-of-contents)
  - [Basic Principles of Semaphores](#basic-principles-of-semaphores)
  - [The Bounded-Buffer Problem](#the-bounded-buffer-problem)
    - [Bounded-Buffer with Semaphores](#bounded-buffer-with-semaphores)
  - [Implementation of Semaphores](#implementation-of-semaphores)
    - [Hardware Synchronization](#hardware-synchronization)
    - [Binary Semaphores](#binary-semaphores)
    - [P and V Operations on General Semaphores](#p-and-v-operations-on-general-semaphores)

## Basic Principles of Semaphores

**Semaphores** are used to systematically solve synchronization problems. They are non-negative integers that can be accessed using only two special operations, P and V:

- V(s): increment s by 1
- P(s): if s>0, decrement s by 1, otherwise wait until s > 0

P and V must guarantee that:

- if several processes simultaneously invoke P(s) or V(s), the operations will occur sequentially in some arbitrary order
- if more than one process is waiting inside P(s) for s to become > 0, one of the waiting processes is selected arbitrarily to complete the P(s) operation

![critical_section](/notes/assets/concurrency/critical_section.PNG)

Semaphores can also be used to solve the critical section problem. A single semaphore that is initialized to 1 can solve the problem for any number of processes

## The Bounded-Buffer Problem

A producer process shares a buffer with a consumer process. The buffer has a fixed number of slots, with the producer filling empty slots with data in increasing order. The consumer follows the producer by removing the data in the same order. This buffer is called a circular buffer as upon reaching the last slot each process continues with the first slot (modulo n)

The solution to this problem must guarantee that:

- consumers do not overtake producers and access empty slots
- producers do not overtake consumers and overwrite full slots

### Bounded-Buffer with Semaphores

Semaphores can be used to solve this problem to ensure that producers do not overtake consumers and vice versa. Two semaphores are used to coordinate the processes. The semaphore f represents the number of full buffer slots. It is incremented upon the producer filling a slot and the consumer emptying one. The semaphore e represents the number of empty slots analogously modified from the producer and consumer:

## Implementation of Semaphores

### Hardware Synchronization

The **test-and-set instruction (TS)** copies a variable into a register and sets the variable to zero in one machine cycle. This has the form TS(R, x) wherein R is a register and x is a memory location. The following operations are performed:

- copy x into R
- set x to 0

A **lock** is a synchronization barrier through which only one process can pass at a time. TS allows for easy locks

### Binary Semaphores

A **binary semaphore** can take only the values 0 or 1. Pb and Vb represent this semaphore's equations and can be implemented directly using the TS instruction:

- Vb(sb): sb = 1
- Pb(sb): do TS(R,sb) while R == 0

**Busy-waiting** is the act of repeatedly executing a loop while waiting for condition to change. This consumes CPU resources and should be avoided when possible

### P and V Operations on General Semaphores

A general semaphore is represented by the variable s and can be implemented using an integer manipulated by P(s) and V(s). To guarantee that only one operation at a time can access and manipulate s, a binary semaphore. The variable s serves to:

- when s >= 0, s represents the value of the semaphore
- whenever s < 0, the value represents the number of processes blocked on the semaphore

To avoid busy-waiting inside P(s) when s falls below 0, the process performs a blocking request
