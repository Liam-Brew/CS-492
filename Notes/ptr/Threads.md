# Threads

## Table of Contents

- [Threads](#threads)
  - [Table of Contents](#table-of-contents)
  - [Thread Concept](#thread-concept)
  - [Thread Control Block](#thread-control-block)
  - [Combined User- and Kernel-level Threads](#combined-user--and-kernel-level-threads)

## Thread Concept

A **thread** is an instance of executing a portion of a program within a process without incurring the overhead of creating and managing separate PCBs

Typical usages of threads include:

- a browser can load an image at the same as waiting for data from the internet. One thread can load the image while the other keeps blocking while retrieving data
- a word processor can run a spell checker while the user is typing. Waiting for each typed character and displaying the data on the screen can be done concurrently by two separate threads
- an internet server can receive many simultaneous requests from different users. If each request is implemented as a thread, many requests can proceed concurrently without holding up others

## Thread Control Block

![tcb](/notes/assets/ptr/tcb.PNG)

A **thread control block (TCB)** holds a separate copy of the dynamically changing information necessary for a thread to execute independently

Threads can be implemented within either a user application or the OS kernel. Depending on what space the thread is implemented in, the opposite space is not aware of the fact that there are individual threads and is only aware of the PCB. Alternatively, for threads implemented in the kernel, the application does not access TCBs and instead uses system calls to interact with the threads

![kernel_and_user_threads](/notes/assets/ptr/kernel_and_user_threads.PNG)

## Combined User- and Kernel-level Threads

Advantages of user-level threads (ULTs) over kernel-level threads (KLTs):

- ULTs don't require kernel interaction and are therefore faster to manage and more can be created
- portability across different OSs

Disadvantages of ULTs:

- ULTs are not visible to the kernel. When one ULT blocks the entire process blocks, reducing concurrency and performance
- ULTs cannot take advantage of multiple CPUs because the process is perceived by the kernel as a single thread

A combined approach is supporting a small number of KLTs. ULTs are then mapped to KLTs based on their needs

![combined_threads](/notes/assets/ptr/combined_threads.PNG)
