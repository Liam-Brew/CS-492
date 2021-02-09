# Why Processes

## Table of Contents

- [Why Processes](#why-processes)
  - [Table of Contents](#table-of-contents)
  - [Virtual CPUs](#virtual-cpus)
    - [Benefits](#benefits)
  - [Modular Structuring](#modular-structuring)

## Virtual CPUs

Using processes to structure an application allows independence from the:

- number of CPUs: the real hardware instance of a CPU is a **physical CPU**. Multiple processes use **time sharing** to create **virtual CPUs** to run on a single physical CPU
- type of CPU: a virtual CPU can either be an abstraction of a physical CPU or software that emulates behavior

![virtual_cpus](https://github.com/Liam-Brew/CS-492/blob/master/Notes/assets/ptr/virtual_cpus.PNG)

### Benefits

The benefit of virtual CPUs are:

- multi-user support: multiple users each represented by one or more separate processes can share the same machine without being aware of each other
- multi-CPU transparency: an application designed to use multiple CPUs will still run (although slower) if only one CPU is available
- portability: applications designed for one type of CPUs can run on other types without being modified or recompiled

![benefits](https://github.com/Liam-Brew/CS-492/blob/master/Notes/assets/ptr/benefits.PNG)

## Modular Structuring

An application can be divided into different processes, which holds several advantages:

- interfaces between processes are easy to understand
- each process can be designed and studied in isolation
- implementation reduces idle time by overlapping the execution of multiple processes
- different processes can utilize separate CPUs, if available, thus speeding up the execution
