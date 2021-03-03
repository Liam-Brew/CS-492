# Real-Time Scheduling

## Table of Contents

- [Real-Time Scheduling](#real-time-scheduling)
  - [Table of Contents](#table-of-contents)
  - [Real-Time Processes](#real-time-processes)
    - [RM Scheduling Algorithm](#rm-scheduling-algorithm)
    - [EDF Scheduling Algorithm](#edf-scheduling-algorithm)
    - [Performance of Real-Time Scheduling Algorithms](#performance-of-real-time-scheduling-algorithms)

## Real-Time Processes

A **real-time process** is a process that receives continuous input that must be processed fast enough to generate instantaneous output. Some examples of this are video streaming, radar data display, and fly-by-wire aircraft

A **period** is a time interval within which each input item must be processed

Real-time processes are scheduled according to several different algorithms

### RM Scheduling Algorithm

The **rate monotonic (RM) algorithm**  schedules process according to the period. The shorter the period, the higher priority. RM scheduling is preemptive

### EDF Scheduling Algorithm

The **earliest deadline first (EDF) algorithm** schedules processes according to the shortest remaining time until the deadline. The shorter the remaining time, the higher priority. EDF scheduling is preemptive

### Performance of Real-Time Scheduling Algorithms

A schedule is **feasible** if the deadlines of all processes can be met

The fraction of CPU time used by a process $i$ is $\frac{T_i}{D_i}$, where $T_i$ is the total CPU time and $D_i$ is the period of process $i$

The **CPU utilization** (U) is the sum of individual fractions of CPU times used by each process. It is calculated using the following formula:

$$ U = \sum_{i=1}^n \frac{T_i}{D_i}$$

If $U = 1$ then the CPU is utilized 100%. A schedule is feasible as long as $U \leq 1$, and no schedule is feasible if $U > 1$

In order to guarantee feasible schedules:

- RM: U must not exceed .7
- EDF: U must not exceed 1
