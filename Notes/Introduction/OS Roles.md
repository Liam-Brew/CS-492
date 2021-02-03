# OS Roles

## Table of Contents

- [OS Roles](#os-roles)
  - [Table of Contents](#table-of-contents)
  - [Hardware/User Gap](#hardwareuser-gap)
  - [OS as an Extended Machine](#os-as-an-extended-machine)
  - [OS as a Resource Manager](#os-as-a-resource-manager)
    - [Multiprogramming](#multiprogramming)
    - [Time-sharing](#time-sharing)
  - [Examples](#examples)
    - [Multiprogramming 1](#multiprogramming-1)
    - [Multiprogramming 2](#multiprogramming-2)
    - [Time-sharing 1](#time-sharing-1)

## Hardware/User Gap

The **operating system** is the software that runs directly on the hardware of a computer and provides the basis on which to build and use applications efficiently and safely

Some tasks that may require OS support are:

- external devices: OS manages interactions
  - ex: inputting a character from a keyboard
- memory partitioning: OS both protects concurrent users of memory and ensures efficiency
  - ex: allocating bytes for a new data structure
  - ex: loading a program into memory
- calling a library function: OS postpones linking and loading of function until it is actually needed
- exiting a program: OS frees up memory and system resources no longer in use
- sleeping: OS selects and runs another program while the requesting program is blocked to avoid wasting resources

Other components of a computer are:

- **CPU**: uses machine instructions to perform operations on registers and memory locations
- **main memory**: sequence of addressable bytes that holds programs and data
- **secondary storage**: used for long-term persistent storage of data
- **I/O**: operated by reading and writing registers of the specific device controllers

## OS as an Extended Machine

![extended_machine](https://github.com/Liam-Brew/CS-492/blob/master/Notes/assets/introduction/extended_machine.PNG)

OSs use **abstraction** (removal of unimportant details to reduce complexity) by creating object hierarchies wherein subservient operations are combined into a single operation at a higher level, hiding details and making the operation easier to use

OSs use **virtualization** (simulating upgraded copies of a real object) to create virtual CPUs, memory, I/O, and other devices to facilitate the work of programmers and users

## OS as a Resource Manager

One of an OS's main tasks is to optimize the use of computational resources to both ensure overall performance and satisfy the requirements of specific applications. Resources can be managed in the following manner:

### Multiprogramming

![multiprogramming](https://github.com/Liam-Brew/CS-492/blob/master/Notes/assets/introduction/multiprogramming.PNG)

**multiprogramming**: several programs are simultaneously active in memory, with execution being switched to maximize resource usage. When a program enters I/O or another phase wherein little CPU power is needed, other programs may instead resume and utilize the CPU in the meantime. Likewise, when an operation is completed, the OS activates computations that can now utilize the device

- overlaps phases of computations an I/O from different processes to better utilize resources, which in turn improves performance
- main objective is to improve resource utilization

### Time-sharing

![time_sharing](https://github.com/Liam-Brew/CS-492/blob/master/Notes/assets/introduction/time_sharing.PNG)

**time-sharing (multitasking)**: extension of multiprogramming wherein the CPU is periodically switched among active computations to share resources among each user. Virtualizes a separate CPU for each computation

- only creates the illusion of simultaneous execution; generally doesn't improve resource utilization

## Examples

### Multiprogramming 1

![multiprogramming_example1](https://github.com/Liam-Brew/CS-492/blob/master/Notes/assets/introduction/multiprogramming_example1.PNG)

**Without multiprogramming**: both phases are sequential since only 1 I/O device can be used

> p1 needs 30 + 70 = 100 time units
>
> p2 needs 40 + 80 = 120 time units
>
Therefore, the total time is 100 + 120 = **220 units**

**With multiprogramming**: the compute phases are sequential because only 1 CPU is available, but the I/O phases can overlap since 2 I/O devices are available

> p1 runs from 0 to 30, followed by the I/O phase from 30 to 100
>
> p2 starts as soon as p1's compute phase terminates and runs from 30 to 70, followed by the I/O phase from 70 to 150
>
Therefore, the total time is 30 + 40 + 80 = **150 units**

### Multiprogramming 2

![multiprogramming_example2](https://github.com/Liam-Brew/CS-492/blob/master/Notes/assets/introduction/multiprogramming_example2.PNG)

> p1 runs from 0 to 40, followed by the I/O phase from 40 to 120. Thus, the I/O device is idle for 40 units (0 to 40)
>
> p2 runs from 40 to 80 but the I/O device is busy until 120. Thus, the CPU is idle for 40 units. The I/O phase runs from 120 to 200
>
> p3 and p4 are also each delayed by 40 units each to await the termination of the previous I/O phase
>
> Finally, the CPU is idle during the last I/O phase

Therefore, the I/O device is idle for only the initial **40 units**. The CPU is idle for 40 + 40 + 40 + 80 = **200 units**

![multiprogramming_example2_chart](https://github.com/Liam-Brew/CS-492/blob/master/Notes/assets/introduction/multiprogramming_example2_chart.PNG)

### Time-sharing 1

![time_sharing_example](https://github.com/Liam-Brew/CS-492/blob/master/Notes/assets/introduction/time_sharing_example.PNG)

> Both processes run concurrently but at half the speed, meaning a processing phase of 1200 units
>
> With 2 I/O devices, both I/O phases overlap and the total is 1200 + 300 = **1500**
>
> With only 1 I/O device, the I/O phases run in sequence and the total is 1200 + 2 * 300 = **1800**
