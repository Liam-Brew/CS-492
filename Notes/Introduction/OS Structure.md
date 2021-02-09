# OS Structure

## Table of Contents

- [OS Structure](#os-structure)
  - [Table of Contents](#table-of-contents)
  - [Hierarchical Structure](#hierarchical-structure)
    - [Monolithic OS](#monolithic-os)
    - [Microkernel OS](#microkernel-os)
    - [Virtual Machines/Hypervisors](#virtual-machineshypervisors)
  - [Processes](#processes)
    - [Memory](#memory)
  - [Address Space](#address-space)
  - [Files](#files)
    - [Special Files](#special-files)
  - [The Shell](#the-shell)
  - [System Calls and Supervisor Calls](#system-calls-and-supervisor-calls)
    - [Execution of a System Call](#execution-of-a-system-call)
  - [Interrupts and Traps](#interrupts-and-traps)
    - [Processing of an Interrupt](#processing-of-an-interrupt)
  - [Examples](#examples)
    - [Interrupt Example](#interrupt-example)

## Hierarchical Structure

**kernel**: minimal set of functions necessary to manage system resources safely and efficiently. Provides the most essential services for memory and device management, computation unit management, and concurrent communication among system activities. Kernel functions may be invoked by any program

**privileged instruction**: performs critical operations that access I/O devices and the CPU's status and control registers. Only executable by the kernel

There are two different options for the CPU mode indicated by the kernel mode bit:

1. **kernel mode**: both privileged and non-privileged instructions may be used
2. **user mode**: only non-privileged instructions may be used

![structure](https://github.com/Liam-Brew/CS-492/blob/master/Notes/assets/introduction/structure.PNG)

### Monolithic OS

The entire OS runs as a single program in kernel mode. This is a collection of procedures linked together into a single large executable binary:

- every function can call the other
- very high performance
- crash affects everything
- hard to understand

![monolithic](https://github.com/Liam-Brew/CS-492/blob/master/Notes/assets/introduction/monolithic.PNG)

### Microkernel OS

Splits the OS up into small, well-defined modules. Only one module runs in kernel mode (the microkernel). The rest run as relatively powerless ordinary suer processes (a bug cannot crash the entire system)

![microkernel](https://github.com/Liam-Brew/CS-492/blob/master/Notes/assets/introduction/microkernel.PNG)

### Virtual Machines/Hypervisors

These are not OSs themselves. They interface with the hardware (below) and provide a hardware interface (above). They run OSs (Linux, Windows, BSD etc.) and unikernels ("libOS", cf.exokernel)

![hypervisor](https://github.com/Liam-Brew/CS-492/blob/master/Notes/assets/introduction/hypervisor.PNG)

## Processes

A program uses resources in its execution (ex: CPU, memory)

A program is only a series of instructions. As resources can't be allocated to that, an **instance** is required of a program in execution. This is a **process**

Resources are allocated/accounted to processes during execution. Permissions are set on a process to control access and restrict sensitive data

### Memory

![memory](https://github.com/Liam-Brew/CS-492/blob/master/Notes/assets/introduction/memory.PNG)

## Address Space

Address space is how the OS manages the memory in a process. The entire address space both includes the kernel space and address spaceS

A basic layout of address space is:

- **stack**: active call data
- **data**: program variables
- **text**: program code

![address_spaces](https://github.com/Liam-Brew/CS-492/blob/master/Notes/assets/introduction/address_spaces.PNG)

## Files

A **file** is an abstraction of a region in a storage device (e.g. a disk)

Files can be read from and written to by providing a position and the amount of data to transfer

Files are maintained in **directories**. A directory keeps an identifier for each file it contains and is a file itself

### Special Files

"Everything is a file": hardware devices are abstracted as files:

- block special files: a disk: ```brw-rw---- 1 root root 8, 2 Dec 4 18:04 /dev/sda2```
- character special files: a serial port: ```crw-rw---- 1 root root 4, 64 Dec 4 18:04 /dev/ttyS0```
- other special files:
  - symbolic links
  - named/anonymous FIFOs (sockets/pipes)
  - everything is a file descriptor

## The Shell

**graphical user interface (GUI)**: provides front-end interaction for the user. Preferred by basic users for its simplicity

**OS shell**: command interpreter that accepts and interprets text command inputs from the user. Preferred by advanced users and developers due to its flexibility and control, especially through the use of shell scripts

## System Calls and Supervisor Calls

![system_calls](https://github.com/Liam-Brew/CS-492/blob/master/Notes/assets/introduction/system_calls.PNG)

**system call**: interface the OS offers to applications to issue service requests

**supervisor call (kernel call)**: privileged instruction that automatically transfers execution control to the kernel. Interfaces the kernel with higher-level software. Similar to a function call with two special features:

1. call switches execution from user mode to kernel mode using the CPU's mode bit
2. to prevent branching to arbitrary kernel locations, the function is not specified by an address but by indirectly using an index into a branch vector. This limits kernel mode to use only well-defined entry points within the kernel

### Execution of a System Call

![supervisor_call](https://github.com/Liam-Brew/CS-492/blob/master/Notes/assets/introduction/supervisor_call.PNG)

1. application, running in user mode, executes a system call S() to request a service from the OS
2. execution transfers to the library function S(), still in user mode
3. S sets up the parameters for the service and executes the privileged instruction
4. execution transfers to the kernel function in kernel mode, which allows the CPU to use privileged instructions
5. when the kernel function completes the requested service, execution returns to the system call function S in user mode
6. the system call function completes the request and returns to the application at the instruction following S()

## Interrupts and Traps

**interrupt**: event that diverts current program execution to a predefined kernel location in order to respond to an event. Triggered by a hardware signal sent to the CPU from an external device. Common usages are:

- completion signal of an I/O operation generated by the I/O device
- implement time-sharing by periodically switching the CPU among concurrent computations. The interrupt is generated by the countdown timer

**trap**: interrupt triggered by the currently executing instruction that causes the OS to abort the current program execution. Ex: dividing by zero, invalid opcodes, and arithmetic overflow

**interrupt handler**: kernel function invoked on interrupt that determines the cause of the interrupt and invokes the appropriate kernel function in response. After, the state of the interrupted computation is restored and control is returned to the instruction following the interrupt

### Processing of an Interrupt

![interrupt](https://github.com/Liam-Brew/CS-492/blob/master/Notes/assets/introduction/interrupt.PNG)

1. application 1 issues a system call S() requesting an I/O operation
2. library function S() invokes the corresponding kernel function using a supervisor call k
3. kernel function, in kernel mode, starts the I/O device using privileged instructions
4. if the data transfer is expected to take a long time, application 1 is blocked
5. the kernel selects another application to run while application 1 is waiting for the I/O device to complete the data transfer
6. when the data transfer is complete, the device issues an interrupt to the CPU, which stops application 2 after the current instruction i and transfers control back to the kernel
7. using the same branch vector as a supervisor call, the kernel transfers control to function m, responsible for handling I/O completion
8. the I/O completion routine returns to S(), which may issue another supervisor call to restart I/O and again block the process to await the I/O completion
9. when application 1 blocks, execution transfers back to the interrupted application 2 and continues with the next instruction i+1. Application 2 is unaware of the interruption used to service application 1

## Examples

### Interrupt Example

![interrupt](https://github.com/Liam-Brew/CS-492/blob/master/Notes/assets/introduction/interrupt_example.PNG)

1. **instruction i**: application 1 is currently running and continues until the first interrupt occurs following instruction i
2. **instruction 0**: execution switches to application 2, which starts with instruction 0
3. **instruction j**: application 2 runs until the second interrupt occurs following instruction j
4. **instruction i+1**: execution switches back to application 1, which continues with the instruction following the interrupt
5. **instruction k**: application 1 runs until the third interrupt occurs following instruction k
6. **instruction j+1**: execution switches back to application 2, which continues with the instruction following the interrupt
