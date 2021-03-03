# Batch Scheduling

## Table of Contents

- [Batch Scheduling](#batch-scheduling)
  - [Table of Contents](#table-of-contents)
  - [Batch Processes](#batch-processes)
    - [FIFO Scheduling Algorithm](#fifo-scheduling-algorithm)
    - [SJF Scheduling Algorithm](#sjf-scheduling-algorithm)
    - [SRT Scheduling Algorithm](#srt-scheduling-algorithm)
    - [Performance of the Algorithms](#performance-of-the-algorithms)
  - [Estimating Total CPU Time](#estimating-total-cpu-time)

## Batch Processes

A **batch process** performs a long-running and repetitive task that does not require any user intervention. They are scheduled according to different algorithm routines

### FIFO Scheduling Algorithm

The **FIFO (First-In-First-Out) algorithm** schedules processes according to the process arrival time, with the early arrivals having a larger priority according to the later ones. FIFO is non-preemptive

### SJF Scheduling Algorithm

The **SJF (Shortest Job First) algorithm** schedules processes according to their total CPU time requirements, with shorter CPU time processes having a higher priority. SLF is non-preemptive

### SRT Scheduling Algorithm

The **SRT (Shortest Remaining Time) algorithm** schedules processes according to the remaining CPU time needed to complete the work. Currently running processes can be interrupted if a new, shorter one arrives. SRT is preemptive

### Performance of the Algorithms

A process's **turnaround time** is the time between its arrival and departure, which is in turn the sum of the total CPU and waiting times. The average turnaround times for different algorithms in an example can be seen below:

![turnaround_times](/notes/assets/scheduling/turnaround_times.PNG)

**Starvation** is the indefinite postponement of a process while other processes are allowed to proceed. Both SJF and SRT can lead to starvation

## Estimating Total CPU Time

Total CPU time can be estimated through the following formula:

$$ S_{n+1} = \alpha T_n + (1- \alpha) S_n$$

wherein:

- $S_n$: last estimate of the total CPU time
- $T_n$: last actually observed total CPU time
- $S_{n+1}$: next total CPU time
- $\alpha$: controls the relative weight of the last observation ($T_n$) versus the history of past predictions ($S_n$)
