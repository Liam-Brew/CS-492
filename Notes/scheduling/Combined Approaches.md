# Combined Approaches

## Table of Contents

- [Combined Approaches](#combined-approaches)
  - [Table of Contents](#table-of-contents)
  - [Two-Tier Approach](#two-tier-approach)
  - [Two-Tier with Floating Priorities](#two-tier-with-floating-priorities)

## Two-Tier Approach

General purpose OSs that run both real-time and batch processes must use a combination of different scheduling algorithms

The simplest approach is to divide processes into two groups. Real-time processes run at the highest priority but due to their short running times can use FIFO. Interactive and batch processes can be scheduled using MLF

![combined_approach](/notes/assets/scheduling/combined_approach.PNG)

## Two-Tier with Floating Priorities

Under this scheme, real-time processes use either FIFO or RR in an ML scheme while interactive and batch processes use a variation of MLF

Every process is assigned a base priority at creation but an increment is added based on the past action the process has taken. Therefore, the current priority depends on the type of process and the process's behavior
