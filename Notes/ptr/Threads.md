# Threads

## Table of Contents

- [Threads](#threads)
  - [Table of Contents](#table-of-contents)
  - [Thread Concept](#thread-concept)
  - [Thread Control Block](#thread-control-block)
    - [Why use the TCB?](#why-use-the-tcb)
  - [Combined User- and Kernel-level Threads](#combined-user--and-kernel-level-threads)
  - [Threads vs. Processes](#threads-vs-processes)
    - [Benefits of Threads](#benefits-of-threads)
    - [POSIX Threads](#posix-threads)

## Thread Concept

A **thread** is an instance of executing a portion of a program within a process without incurring the overhead of creating and managing separate PCBs. They are independent executions of a process

Typical usages of threads include:

- a browser can load an image at the same as waiting for data from the internet. One thread can load the image while the other keeps blocking while retrieving data
- a word processor can run a spell checker while the user is typing. Waiting for each typed character and displaying the data on the screen can be done concurrently by two separate threads
- an internet server can receive many simultaneous requests from different users. If each request is implemented as a thread, many requests can proceed concurrently without holding up others

## Thread Control Block

A **thread control block (TCB)** holds a separate copy of the dynamically changing information necessary for a thread to execute independently

![tcb_pcb](/notes/assets/ptr/tcb_pcb.PNG)

Similar to a Process Table, the kernel also manages a Thread Table wherein the TCBs are stored. Information  on program execution, such as the program counter, CPU registers, scheduling information, and pending IO information are stored in the TCB

Other information such as memory management and accounting information, the PID, and file descriptors is stored in the PCB. This information is common across all the threads. Each TCB contains a reference to the PCB

![tcb](/notes/assets/ptr/tcb.PNG)

Threads can be implemented within either a user application or the OS kernel. Depending on what space the thread is implemented in, the opposite space is not aware of the fact that there are individual threads and is only aware of the PCB. Alternatively, for threads implemented in the kernel, the application does not access TCBs and instead uses system calls to interact with the threads

![kernel_and_user_threads](/notes/assets/ptr/kernel_and_user_threads.PNG)

### Why use the TCB?

The TCB is used so that different parts of the program code, each representing a thread, can be executed concurrently. Also, the same parts of the program code execute at the same time but with different execution states. This means that these threads have independent current instructions and work with different data

## Combined User- and Kernel-level Threads

Advantages of user-level threads (ULTs) over kernel-level threads (KLTs):

- ULTs don't require kernel interaction and are therefore faster to manage and more can be created
- portability across different OSs

![user_threads](/notes/assets/ptr/user_threads.PNG)

Disadvantages of ULTs:

- ULTs are not visible to the kernel. When one ULT blocks the entire process blocks, reducing concurrency and performance
- ULTs cannot take advantage of multiple CPUs because the process is perceived by the kernel as a single thread

Advantages of KLTs:

- no run-time system needed in each process
- no thread table in each process
- blocking system calls are not a problem (the kernel scheduler can schedule another thread)
- supports real parallelism in multicore/multiprocessor systems

A combined approach is supporting a small number of KLTs. ULTs are then mapped to KLTs based on their needs

![combined_threads](/notes/assets/ptr/combined_threads.PNG)

## Threads vs. Processes

The initialization of each process takes time, as each process requires its own PCB and therefore memory allocation

Threads require a process to serve as a "container". One process may contain one or multiple threads. Therefore, many threads can be found within one process, requiring the overhead of only one PCB

A thread is an independent sequential execution stream within a process. Programs use one or multiple threads per process

### Benefits of Threads

All threads in the same process use the same address space. This allows threads to talk to each other

![threads_vs_processes](/notes/assets/ptr/threads_vs_processes.PNG)

### POSIX Threads

![posix_threads](/notes/assets/ptr/posix_threads.PNG)
