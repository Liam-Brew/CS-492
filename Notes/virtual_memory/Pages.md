# Pages

## Table of Contents

- [Pages](#pages)
  - [Table of Contents](#table-of-contents)
  - [Principles of Paging](#principles-of-paging)
  - [Logical and Physical Address](#logical-and-physical-address)
  - [Address Translation](#address-translation)
  - [Internal Fragmentation and Bound Check](#internal-fragmentation-and-bound-check)
  - [Principles of Virtual Memory](#principles-of-virtual-memory)
    - [Demand Paging](#demand-paging)
    - [Page Replacement](#page-replacement)
    - [Paging of Tables](#paging-of-tables)

## Principles of Paging

**Page**: fixed-size continuous block of a logical address space identified by a number (page number)

**Page frame**: fixed-size continuous block of physical memory identified by a number (page frame number). The page frame is the smallest unit of data for memory management and may contain a copy of any page

**Page table**: array that tracks which pages of a given logical address space reside in which page frames. Each table entry corresponds to one page and contains the starting address of the frame containing the page

![paging](/notes/assets/virtual_memory/paging.PNG)

## Logical and Physical Address

The size of an address space is $2^n$, where n is the number of bits necessary to form addresses for the entire space

The size of a page is $2^k$, where k is the number of bits needed to address every word on a page

Therefore the number of pages is determined by $2^{n-k}$

- $n-k$ specifies the page number p
- k specifies an offset w within the page

If given the logical address and the page size, the page number can be determined by dividing the LA by the page size. The offset is the remainder of this division

With m bits in a physical address, $2^m$ total physical addresses can be formed. The frame size must be equal to the page size, therefore k bits are needed to address every word in a frame. The number of frames is determined by $2^{m-k}$

![logical_physical](/notes/assets/virtual_memory/logical_physical.PNG)

## Address Translation

Logical addresses take the form (p,w) wherein p is a page number and w is an offset. Likewise, physical addresses take the form of (f,w) wherein f is a frame number and w is an offset. Logical addresses are translated to their physical counterparts through the following steps:

1. given logical address (p,w) access the page table entry corresponding to page p
2. read the frame number, f, of the frame containing p
3. combine f with the offset w to find the physical address (f,w) corresponding to the logical address (p,w)

The physical address is obtained by multiplying the frame number f by the page size and adding the offset w

## Internal Fragmentation and Bound Check

**Internal fragmentation** is the loss of usable memory space due to the mismatch between the page size and the size of a program. This creates a hole at the end of the program's last page

## Principles of Virtual Memory

There are several strategies which determine how to load required pages into memory

### Demand Paging

**Virtual memory (VM)**: collection of address spaces, each of which may not exceed the size of physical memory

**Demand paging**: principle of loading a page into memory only when the page is needed, rather than at the start of the execution

**Present bit**: binary flag in each page table entry that indicates whether the page is currently in memory

**Page fault**: interrupt that occurs when a program attempts to reference a non-resident page. The interrupt triggers the OS to find the page on the disk, copy the page into frame, and set the present bit to 1

![demand_paging](/notes/assets/virtual_memory/demand_paging.PNG)

### Page Replacement

**Page replacement**: pages in memory are overwritten with a different page loaded from the disk when needed

**Modified-bit (m-bit)**: binary flag in each page table entry that indicates whether the corresponding page has been modified during execution. Automatically set to 1 by any instruction that stores data into the page

### Paging of Tables

As VM can create address spaces much larger than available physical memory, and few programs make use of such large programs, a table can be divided into individual pages and brought into memory as needed to reduce wastage

![table_paging](/notes/assets/virtual_memory/table_paging.PNG)
