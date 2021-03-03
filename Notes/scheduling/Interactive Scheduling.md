# Interactive Scheduling

## Table of Contents

- [Interactive Scheduling](#interactive-scheduling)
  - [Table of Contents](#table-of-contents)
  - [Interactive Processes](#interactive-processes)
    - [RR Scheduling Algorithm](#rr-scheduling-algorithm)
    - [ML Scheduling Algorithm](#ml-scheduling-algorithm)
    - [MLF Scheduling Algorithm](#mlf-scheduling-algorithm)
    - [Performance of Interactive Scheduling Algorithms](#performance-of-interactive-scheduling-algorithms)

## Interactive Processes

An **interactive process** communicates with the user by receiving commands or data from input and generating corresponding output. The primary goal when scheduling these processes is to quickly respond to input. There exist multiple ways to organize this scheduling

### RR Scheduling Algorithm

The **round-robin (RR) algorithm** uses a single queue of processes to organize scheduling. A process's priority is based on its position in the queue. The head of the queue is allowed to run for *Q* time units before it is moved back to the end of the queue

*Q* represents a **time quantum**, which is a small amount of time during which the process is allowed to use the CPU

### ML Scheduling Algorithm

The **multilevel (ML) algorithm** maintains a separate queue of processes at each priority level. Within each level, processes are scheduled according to RR

### MLF Scheduling Algorithm

The **multilevel feedback (MLF) algorithm** addresses the problems of starvation and fairness by

- using a different time quantum at each priority level
- changing the priority of every process dynamically

Newly arrived processes enter the highest-priority queue and is allowed to run for *Q* time unites. When *Q* is exceeded, the process is moved to the next lower queue *N-1* and is allowed to run for *2Q* time units, with the quantum size being doubled for each decreasing priority level. This means that at priority level *L*, the maximum allowed time is $2^{N-L}Q$ time units

MLF favors short-running processes, with longer processes gradually migrating to lower priority levels

### Performance of Interactive Scheduling Algorithms

The **response time** is the elapsed time from the submission of an input request until the response begins to arrive. The response time increases as more processes time-share the CPU

![response_time](/notes/assets/scheduling/response_time.PNG)
