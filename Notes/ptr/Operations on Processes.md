# Operations on Processes

## Table of Contents

- [Operations on Processes](#operations-on-processes)
  - [Table of Contents](#table-of-contents)
  - [Process Creation Hierarchy](#process-creation-hierarchy)
  - [Process Creation](#process-creation)
    - [Process Destruction](#process-destruction)

## Process Creation Hierarchy

![process_creation_hierarchy](/notes/assets/ptr/process_creation_hierarchy.PNG)

The process creation hierarchy is a graphical representation of the dynamically changing parent-child relationships among all processes. The creation hierarchy changes each time a process is created or destroyed

## Process Creation

The **create process function** allocates a new PCB, fills it with initial values and links it with other existing PCB structures

```c
create(state0, mem0, sched0, acc0) { 
   p = allocate new PCB
   p.cpu_state = state0
   p.memory = mem0
   p.scheduling_information = sched0
   p.accounting_information = acc0
   p.process_state = ready
   p.parent = self
   insert p into self.children
   p.children = NULL
   p.open_files = NULL
   p.other_resources = NULL
   insert p into RL
   scheduler()
}
```

### Process Destruction

The **destroy process function** destroys a process by freeing the PCB data structure and removing references to the PCB from the system

```c
destroy(p) {
   for all c in p.children destroy(c) 
   remove p from RL or waiting list
   remove p from parent's list of children
   release p.memory 
   release p.other_resources 
   close all p.open_files 
   deallocate PCB
}
scheduler()
```
