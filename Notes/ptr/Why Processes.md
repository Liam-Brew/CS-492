# Why Processes

## Table of Contents

- [Why Processes](#why-processes)
  - [Table of Contents](#table-of-contents)
  - [Virtual CPUs](#virtual-cpus)
    - [Benefits](#benefits)
  - [Modular Structuring](#modular-structuring)
    - [Multiprogramming Model](#multiprogramming-model)
    - [Sequential Processes](#sequential-processes)
    - [Time Sharing](#time-sharing)

## Virtual CPUs

Using processes to structure an application allows independence from the:

- number of CPUs: the real hardware instance of a CPU is a **physical CPU**. Multiple processes use **time sharing** to create **virtual CPUs** to run on a single physical CPU
- type of CPU: a virtual CPU can either be an abstraction of a physical CPU or software that emulates behavior

![virtual_cpus](/notes/assets/ptr/virtual_cpus.PNG)

### Benefits

The benefit of virtual CPUs are:

- multi-user support: multiple users each represented by one or more separate processes can share the same machine without being aware of each other
- multi-CPU transparency: an application designed to use multiple CPUs will still run (although slower) if only one CPU is available
- portability: applications designed for one type of CPUs can run on other types without being modified or recompiled

![benefits](/notes/assets/ptr/benefits.PNG)

## Modular Structuring

An application can be divided into different processes, which holds several advantages:

- interfaces between processes are easy to understand
- each process can be designed and studied in isolation
- implementation reduces idle time by overlapping the execution of multiple processes
- different processes can utilize separate CPUs, if available, thus speeding up the execution

### Multiprogramming Model

Each program *A*, *B*, *C*, and *D* represents a unique process. They are being run on a single core/thread CPU:

![multiprogramming_process](/notes/assets/ptr/multiprogramming_process.PNG)

### Sequential Processes

These processes are each being run concurrently on a different core and are able to communicate with each other. However, this provides wasting of CPU resources, such as when a process blocks for IO

![sequential_processes](/notes/assets/ptr/sequential_processes.PNG)

### Time Sharing

These four processes run on a single core CPU and only one is active at once. However, the processes are run in a way that appears to be simultaneous. Process execution is switched when the current process blocks

![time_sharing](/notes/assets/ptr/time_sharing.PNG)
