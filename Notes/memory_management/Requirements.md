# Requirements

## Table of Contents

- [Requirements](#requirements)
  - [Table of Contents](#table-of-contents)
  - [Logical vs. Physical Memory](#logical-vs-physical-memory)
  - [Program Transformations](#program-transformations)
  - [Relocation and Address Binding](#relocation-and-address-binding)
    - [Dynamic Implementation via Relocation Registers](#dynamic-implementation-via-relocation-registers)
  - [Free Space Management](#free-space-management)
  - [Managing Insufficient Memory](#managing-insufficient-memory)
    - [Memory Fragmentation and the 50% Rule](#memory-fragmentation-and-the-50-rule)
    - [Swapping and Memory Compaction](#swapping-and-memory-compaction)
    - [Dynamic Linking and Sharing](#dynamic-linking-and-sharing)

## Logical vs. Physical Memory

**Physical memory (RAM)**: hardware structure consisting of linear sequence of words that hold a program during execution

**Word**: fixed-size unit of data (typically 2 bytes)

**Physical address**: integer range [0 : n-1] that identifies a word in a physical memory of size n. A memory of size n requires an address size of k bits where $n = 2^k$

**Logical address space**: abstraction of physical memory consisting of a sequence of imaginary memory locations in a range [0 : m-1], where m is the size of the logical address space

**Logical address**: integer in the range [0 : m-1] that identifies a word in a logical address space. A logical address is mapped to physical memory prior to execution

![logical_physical_mem](/notes/assets/memory_management/logical_physical_mem.PNG)

## Program Transformations

**Source module**: program written in a symbolic language, such as C or assembly, that must be translated by a compiler into machine code

**Object module**: machine-language output of a compiler generated from a source module

**Load module**: program in a form that is ready to be loaded into main memory and executed

![program_transformations](/notes/assets/memory_management/program_transformations.PNG)

## Relocation and Address Binding

**Relocation**: act of moving a program component from one address space to another. May be between two logical address spaces or from a logical to a physical address space

**Static relocation**: binds all logical addresses to a physical address prior to execution

**Dynamic relocation**: postpones the binding of a logical address to a physical address until the addressed item is accessed during execution

![static_dynamic_relocation](/notes/assets/memory_management/static_dynamic_relocation.PNG)

### Dynamic Implementation via Relocation Registers

**Relocation register**: contains the physical starting address of a program in memory

## Free Space Management

Different strategies for managing free space are:

- **first-fit**: always starts the search from the beginning of the list and allocates the first hole large enough to accommodate the request
- **next-fit**: starts each search at the point of the last allocation
- **best-fit**: searches the entire list and chooses the smallest hole large enough to accommodate the request
- **worst-fit**: takes the opposite approach from best-fit by always choosing the largest available hole for any request

## Managing Insufficient Memory

### Memory Fragmentation and the 50% Rule

**External memory fragmentation**: loss of usable memory space due to holes between allocated blocks of variable sizes

**50% rule**: if the probability of finding an exact match for a request approaches 0, one third of all memory partitions are holes and two thirds are occupied blocks

### Swapping and Memory Compaction

**Swapping**: temporary removal of a module from memory. The module is saved on a disk and moved back to memory. Dynamic relocation ensures that the module can be placed in a different location without modification

**Memory compaction**: systematic shifting of modules to consolidate multiple disjoint holes into one larger hole

### Dynamic Linking and Sharing

**Linking**: act of resolving external references among object modules. Can be done both statically (before loading) as well as dynamically (while the program is executing)

**Sharing**: act of linking the same copy of a module to multiple other modules. Used to improve memory utilization by allowing processes to share common routines and services