# Disk Block Allocation

## Table of Contents

- [Disk Block Allocation](#disk-block-allocation)
  - [Table of Contents](#table-of-contents)
  - [Disk Blocks](#disk-blocks)
    - [Contiguous Allocation](#contiguous-allocation)
    - [Linked Allocation](#linked-allocation)
    - [Clustered Allocation](#clustered-allocation)
    - [Using a File Allocation Table](#using-a-file-allocation-table)
    - [Indexed Allocation](#indexed-allocation)
    - [Free Space Management](#free-space-management)

## Disk Blocks

**Disk block**: fixed sequence of bytes on the disk that can only be accessed using low-level read and write operations. Disk blocks can be allocated by using the following strategies

### Contiguous Allocation

**Contiguous block allocation scheme**: every file is mapped into a contiguous sequence of disk blocks. The FCB points to the first disk block, with the file length (also maintained by the FCB) determines how many blocks are occupied

The advantages of this method are:

- fast sequential data access due to no requirement for seek operations
- fast direct access because a target block can be computed using the file position and block length

The drawbacks of this method are:

- disk fragmentation over time
- inability to expand the file (entire file must be copied over)
- difficulty in deciding how much space to allocate to a new file

### Linked Allocation

**Linked block allocation scheme**: blocks containing a file may be scattered throughout the disk. The FCB points to the first block and each block logically points to the next one

The advantage of this method are:

- easy expandability of a file by linking additional blocks to the last block

The disadvantages are:

- slower sequential access as each block requires a seek operation
- inability to perform direct access since the location of the target block cannot be calculated, instead retiring a link
- decreased reliability of the disk. If one block becomes damages, the rest of the file cannot be found
- considerable waste of space since every block needs a pointer to the next

### Clustered Allocation

**Clustered block allocation**: links together sequences of contiguous blocks. The last block of any cluster points to the beginning of the next cluster, and also records the number of blocks belonging to the next cluster to facilitate direct access within a cluster

The advantages of this approach are:

- easy expandability of a file. If the last block is free, then the last cluster is extended as with the contiguous allocation
- reduced number of pointers since only the last block of a cluster contains a pointer
- faster sequential access than linked allocation as blocks within a cluster don't require seek operations

Its disadvantages are:

- slower sequential access than contiguous
- inability to perform direct access across clusters as the location of the target block cannot be computed. Pointers must be followed until the cluster containing the target block is reached

### Using a File Allocation Table

**File allocation table (FAT)**: array where each entry corresponds to a disk block. FAT tracks which disk blocks belong to a file through the linking of blocks in a chain of indices

FAT provides all the advantages of linked allocation, in addition to the following:

- more efficient sequential access due to no seek operations being necessary on pointers
- direct access is possible as the location of the desired block can be found by scanning the chain of indices in the fat

### Indexed Allocation

**Indexed block allocation scheme**: file blocks may reside anywhere on the disk. An index table is provided for each file, which tracks the blocks belonging to that file

The advantages of this strategy are:

- fast sequential and direct access
- efficient expansion of a file by adding a new block number to the index table

The main drawback is:

- necessity to decide how large the index table should be
  - small tables limit the max file size
  - large tables waste disk space as most files wont be that large

### Free Space Management

**bitmap**: data structure where each bit represents one bit block. A 1 indicates that the block is allocated and a 0 indicates that it is free