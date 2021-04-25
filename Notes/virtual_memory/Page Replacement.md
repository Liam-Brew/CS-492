## Page Replacement

## Table of Contents

- [Page Replacement](#page-replacement)
- [Table of Contents](#table-of-contents)
- [Page Replacement with Fixed Numbers of Frames](#page-replacement-with-fixed-numbers-of-frames)
  - [Aging Page Replacement Algorithm](#aging-page-replacement-algorithm)
  - [Second-chance Page Replacement Algorithm](#second-chance-page-replacement-algorithm)
  - [Third-chance Page Replacement Algorithm](#third-chance-page-replacement-algorithm)
- [Page Replacement with Variable Numbers of Frames](#page-replacement-with-variable-numbers-of-frames)
  - [Working Set Page Replacement Algorithm](#working-set-page-replacement-algorithm)
  - [Page-Fault-Frequency Replacement Algorithm](#page-fault-frequency-replacement-algorithm)
- [Time and Space Efficiency of Virtual Memory](#time-and-space-efficiency-of-virtual-memory)
  - [Overhead of Demand Paging](#overhead-of-demand-paging)
  - [Load Control](#load-control)

## Page Replacement with Fixed Numbers of Frames

**Reference string**: sequence of page numbers referenced by an executing program during a given time interval. These are used to compare different page replacement algorithms by counting the number of page faults generated

Several algorithms exist to manage page replacements:

**Optimal page replacement algorithm**: selects the page that will not be referenced for the longest time in the future. This algorithm is unrealizable as future references are unknown at runtime. However, it provides a useful lower bound on the number of page faults. This bound can be used to compare the effectiveness of other algorithms

**FIFO page replacement algorithm**: selects the page that has been resident in memory for the longest time. This algorithm is easy to implement and takes advantage of locality through the use of sequential execution

**Least-recently-used (LRU) page replacement algorithm**: selects the page that has not been referenced for the longest time. Requires a queue of n, wherein n is the number of memory frames. The queue contains the numbers of all resident pages. Page referencing protocols depend on the type of page:

- **resident page**: page p is moved from the head to the end of the queue, sorting it from least recently used to most recently used
- **non-resident page**: page p is moved to the end of the queue and hte least recently referenced page q at the head of the queue is removed. Page p replaces q in memory

The LRU algorithm can be approximated through several methods:

### Aging Page Replacement Algorithm

This algorithm does not maintain pages sorted in the exact LRU order, but instead groups together pages referenced during a period of d consecutive references. Each period is represented by 1 bit in a periodically shifting aging register

**referenced bit (r-bit)**: bit associated with a page and is set automatically by the hardware when the page is referenced by any instruction

**aging register**: associated with a page and is shifted periodically to the right by one bit. Unless the most significant bit is set to 1, the page is aging in the sense that the associated register value is steadily decreasing

When a page fault occurs:

- select the page with the smallest aging register value for replacement
- if multiple pages have the same smallest value then select one at random

### Second-chance Page Replacement Algorithm

This algorithm uses the r-bit to divide all pages into two categories: those recently referenced and those not. A page is selected from the not-recently referenced category

At a page fault, the algorithm repeats the following steps until a page is selected for replacement:

1. if the pointer is at a page p with r=0, then select p and advance the pointer to the next page
2. otherwise, reset r to 0, advance the pointer to the next page, and continue the search

### Third-chance Page Replacement Algorithm

This algorithm is a refinement of the second-chance algorithm and is also known as the **not-recently-used page replacement algorithm (NRU)**. This algorithm divides pages into 4 categories based on the 4 possible combinations of the r-bit and the m-bit

At a page fault, the algorithm repeats the following steps until a page is selected for replacement:

1. if the pointer is at a page p with r=0 and m=0 then select p and advance the pointer to the next page
2. otherwise reset the bits according to the following table, advance the pointer to the next page, and continue the search

## Page Replacement with Variable Numbers of Frames

**Optimal working set**: the set of pages that will be needed in the immediate future and thus should be resident. Its size varies with the program's behavior:

- when execution is highly localized in long tight loops and static data structures, the set contains a small number of repeated page numbers
- when execution involves many branches or highly dynamic data structures, the set contains many different page numbers

### Working Set Page Replacement Algorithm

This algorithm uses a trailing window os size d superimposed on the RS to determine the size and composition of the working set at time t

**Working set (WS)**: set of pages referenced during the past d memory operations preceding time t

At each memory referenced the following steps are followed:

1. advance the sliding window by 1 to include the current reference
2. keep resident only the pages that appear in the window

### Page-Fault-Frequency Replacement Algorithm

This algorithm is similar to the working set algorithm, the difference being that this algorithm adjusts the current resident set based on how frequently consecutive page faults occur. A constant d is used to determine if the page fault frequency is too high, thus meaning that the resident set should be increased. If the page fault frequency is acceptable then the resident set can be reduced by removing some of the pages not referenced recently

When a page fault occurs:

1. add the currently referenced page causing the page fault to the resident set
2. when the time since the previous page fault is greater than d, remove all pages from the resident set that have not been referenced since the previous page fault

## Time and Space Efficiency of Virtual Memory

### Overhead of Demand Paging

**Page fault rate**: number of page faults, f, occurring during a number of memory references, t. The page fault rate can be expressed as $P = f/t$, where $0 \leq P \leq 1$. P=1 means that every memory reference results in a page fault, and P=0 means that no page faults occur

**Effective access time**: average time to access memory in the presence of page faults. The  effective access time, E, depends on the frequency of page faults:
$$ E = (1-P) * m + P *s$$

where m is the time to access physical memory and S is the time to process a page fault

### Load Control

**load control**: activity of determining how many processes should be running concurrently at any given time to maximize overall system performance

**thrashing**: execution state during which most of the time is spent on moving pages between the memory and the disk while the CPU is mostly idle and no process is making any real progress