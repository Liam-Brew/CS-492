# Process Control Block

## Table of Contents

- [Process Control Block](#process-control-block)
  - [Table of Contents](#table-of-contents)
  - [Contents of the PCB](#contents-of-the-pcb)
  - [Organizing PCBs](#organizing-pcbs)
    - [Avoiding Linked Lists](#avoiding-linked-lists)
  - [Managing PCBs](#managing-pcbs)
  - [Creating New Processes](#creating-new-processes)

## Contents of the PCB

The Process Table consists of multiple processes. Each entry is represented by a Process Control Block

![pcb_composition](/notes/assets/ptr/pcb_composition.PNG)

The PCB is the instantiation of a process. When a process is created, the OS assigns every process a unique identifier *p* that could be a pointer to a PCB structure or an index to an array of PCBs

| PCB Field       | Explanation                                                                |
| --------------- | -------------------------------------------------------------------------- |
| CPU_state       | the current state of the CPU when the process is stopped                   |
| process_state   | stores *p*'s current state (running, ready, or blocked)                    |
| memory          | describes the area of memory assigned to p (either main or virtual memory) |
| scheduling_info | information used by the scheduler to determine when p should run           |
| accounting_info | information for resource accounting and billing purposes                   |
| open_files      | tracks files currently opened by *p*                                       |
| other_resources | tracks resources, such as printers, that *p* has requested and acquired    |
| parent          | the process that created *p*                                               |
| children        | processes created by *p*                                                   |

## Organizing PCBs

Two ways exist to organize PCBs:

1. array of structures. The PCBs are marked as free or allocated which eliminates the need for dynamic memory management. Drawback of a lot of wasted memory spaces
2. array of pointers. The PCB memory is dynamically allocated. Less wasted space, but more overhead due to dynamic memory management

### Avoiding Linked Lists

The dynamic memory allocation of linked lists is costly. To combat this, instead of a separate linked list in the parent's PCB, the links are distributed over the child PCBs such that each points to the immediate younger sibling and immediate older sibling. This replaces the *parent* and *children* fields with four new ones:

1. *parent*: points to *p*'s single parent as before
2. *first_child*: points to *p*'s first child
3. *younger_sibling*: points to the sibling of *p* created immediately following *p*
4. *older_sibling*: points to the sibling of *p* created immediately prior to *p*\

This method requires no dynamic memory management

## Managing PCBs

**waiting list**: associated with every resource. Contains all the processes that are blocked on that resource because the resource is not available

**ready list**:  contains all processes that are in the ready state and thus are able to run on the CPU. Sorts processes by their importance, expressed by an integer value called the *priority*

## Creating New Processes

- *fork()*: creates a new process
- *execve()*: installs a new program in the process

Both *fork()* and *execve()* are system calls

Process creation is done by the *clone()* system call

- clone can create new processes or new threads
- creates a new task (new process means that the address space is not shared)
- Linux assigns PID based on clone's parameters
