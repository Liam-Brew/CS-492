# Process Concept

## Table of Contents

- [Process Concept](#process-concept)
  - [Table of Contents](#table-of-contents)
  - [Process States and Transitions](#process-states-and-transitions)
    - [Basic States](#basic-states)
    - [Extra States](#extra-states)
  - [Context Switch](#context-switch)
    - [State Transitions](#state-transitions)

A **process** is an instance of a program being executed by an OS, with the OS itself being a collection of processes

A **process control block (PCB)** is a data structure that holds information for a process. It is created and maintained by the OS, not the process itself. It includes the following:

- current instruction address
- execution stack
- resources used by the process
- program being executed

![pcb](/notes/assets/ptr/pcb.PNG)

## Process States and Transitions

### Basic States

![basic_states](/notes/assets/ptr/basic_states.PNG)

The following three basic states exist for most OSs:

1. **running state**: the process has all necessary resources and the CPU is executing the program's instructions
2. **ready state**: the process has all necessary resources to run but the CPU is currently unavailable
3. **blocked state**: the process is waiting on a currently unavailable resource

### Extra States

![other_states](/notes/assets/ptr/other_states.PNG)

There are additional process states that may include the following:

- **new state**: a newly created process is placed in this state before it is allowed to compete for the CPU
- **terminated state**: execution can no longer continue but before the PCB is deleted
- **suspended state**: process is not running even though all resource and the CPU are available

## Context Switch

A **context switch** is the transfer of control from one process to another. When a process stops executing to allow another one to resume, the OS saves information about it that is restored when the process resumes running

The **CPU state** consists of all intermediate values held in CPU registers and hardware flags at the time of interruption

![context_switch](/notes/assets/ptr/context_switch.PNG)

### State Transitions

The following state transitions are possible:

- running -> ready
- running -> suspended
- running -> blocked (resource request)
- ready -> running (OS scheduler)
- suspended -> ready (reactivate)
- blocked -> ready
