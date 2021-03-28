# Files

## Table of Contents

- [Files](#files)
  - [Table of Contents](#table-of-contents)
  - [File Concept](#file-concept)
  - [File Types](#file-types)
  - [File Directories](#file-directories)
    - [Tree-structured Directory Hierarchy](#tree-structured-directory-hierarchy)
    - [Operations on Directories](#operations-on-directories)
    - [Directed Acyclic Directory Structure](#directed-acyclic-directory-structure)
    - [General Graph Structure with Symbolic Links](#general-graph-structure-with-symbolic-links)
    - [Contents of File Directories](#contents-of-file-directories)
  - [Operations on Files](#operations-on-files)
    - [Create/Destroy](#createdestroy)
    - [Open](#open)
    - [Read](#read)
    - [Write](#write)
    - [Seek](#seek)
    - [Close](#close)

## File Concept

**File system (FS)**: management system and hierarchy of files

**File**: named collection of information located on secondary storage. The FS provides high-level operations for users to access files without knowing the specific details of the underlying storage mediums

**Record**: structure of related data items identified within a file by a record number or unique key field. An example is a student ID distinguishing all student records in a file

**Access method**: set of operations provided by the OS as part of the user interface to access files. The most common method is sequential. The FS maintains the current position within the file and each read/write accesses the next n bytes or record in the file

![fs](/notes/assets/file_system/fs.PNG)

## File Types

**Metadata**: information about the format and organization of a file's data, generally stored in the header of the file

**File header**: portion of the file preceding the actual data and is visible to only the FS itself

**Magic number**: short sequence of characters at the start of the file header which identifies the file type. This type determines which program are allowed to access and interpret the file

**File extension**: sequence of characters following the file name. They are visible and can be changed by the user, and therefore only support a weak version of file typing

## File Directories

**File directory (folder)**: special-purpose file that records information about other files and other directories. The directory consists of a set of entries, each containing the name of a file followed by other information necessary to access it

### Tree-structured Directory Hierarchy

**Tree-structured directory hierarchy**: collection of directories organized such that:

1. every directory points to 0 or more files or directories at the next lower level
2. every file and directory except the root is pointed to by exactly one parent directory at the next higher level

**Root**: the highest level directory which does not have its own parent directory

Under this approach, every file and directory has a unique ID assigned by the FS at the time of creation. This ID is only used internally by the FS, with ASCII names being assigned by users to differentiate files

**Absolute path name**: concatenation of the directory and file names leading from the root to the file f

**Relative path name**: concatenation of file names starting with the current directory

![tree_directory](/notes/assets/file_system/tree_directory.PNG)

### Operations on Directories

| Operation | Description                                                                                                                      |
| --------- | -------------------------------------------------------------------------------------------------------------------------------- |
| change    | change the current working directory to another specified by path name                                                           |
| create    | create a bew baned directory in the current working directory, which points to the new directory                                 |
| delete    | delete directory d specified by a path name. Delete also all files and directories reachable using any path name starting from d |
| move      | move a file or directory from one directory to another                                                                           |
| rename    | change the name of a file or directory specified by a path name                                                                  |
| list      | list the file names and optionally other attributes of all files contained in a directory specified by path name                 |
| find      | find a file or directory specified by a name                                                                                     |

### Directed Acyclic Directory Structure

**Directed acyclic directory hierarchy**: organizes directories such that any directory at a given level may point to zero or more files or directories at lower levels, but also permits any file or directory to have more than one parent level

To allow for multiple parents, the FS provides a ```link``` operation which links between two files identified by paths. This takes the form ```link path1 path2```

**Reference count**: non-negative integer associated with a file that indicates how many directories point to said file

![directed_acyclic_directory](/notes/assets/file_system/directed_acyclic_directory.PNG)

### General Graph Structure with Symbolic Links

**Symbolic link (shortcut)**: directory entry that points to a file or directory similar to a regular entry, but when deleted only the link is removed (not the file itself)

### Contents of File Directories

Typical file attributes include:

- size
- type
- location
- protection (access levels)
- use (date and times)

**File control block (FCB)**: data structure associated with a filename that contains all relevant attributes of the file. FCBs are stored apart from file directories and are pointed to by corresponding directory entries. These are called i-nodes in Unix

![file_directory_entry](/notes/assets/file_system/file_directory_entry.PNG)

The FS must be able to efficiently:

1. search directories for a file name
2. find and allocate an entry upon file creation
3. free an entry when a file is deleted
4. change the file name

## Operations on Files

### Create/Destroy

**Create file**: FS allocates and initializes a new file control block (FCB) as well as a free directory entry which contains the file's name and a pointer to the FCB

**Destroy file**: FS marks the file's disk blocks as free for reuse by the machine

### Open

**Open file table (OFT)**: data structure that tracks all files currently in use to facilitate efficient access to and manipulation of files

**Open file**: prepares file for access and manipulation by retrieving relevant information from the FCB and storing it as an entry of the OFT. Steps are:

1. verify the invoking process has the right to access and operate on the file
2. find and allocate a free entry in the OFT
3. allocate read/write buffers as necessary for the given type of file access
4. copy relevant information from the FCB to the OFT entry, including the file length and location on disk
5. initialize other information in the OFT entry, such as the initial position of a sequentially accessed file
6. return the index of the allocated OFT entry to the calling process

### Read

**Read file**: copes data from an open file to a specified area in main memory. This data may be accessed either directly from one record at a time or sequentially by specifying the number of bytes to be read next. It has the form ```read i, m, n```:

- i is an OFT index corresponding to an open file
- n specifies the number of bytes to be read
- m is a location oin memory where the data is to be stored

### Write

**Write file**: copies data from an area in main memory to a specified open file. It has the form ```write i, m, n``` where:

- i is an OFT index corresponding to an open file
- n specifies the number of bytes to write
- m is the starting location in memory from which the data is to be copied

### Seek

**Seek file**: moves the current position of an open file to a new specified position. It has the form ```seek i, k``` where:

- i is an OFT index corresponding to an open file
- k specifies the new position within the file

### Close

**Close file**: reverses the effects of the open operation by saving the current state of the file in the FCB and freeing the OFT entry. The steps it performs are:

1. if the current content of the r/w buffer is modified, save the buffer in the corresponding block on the disk
2. update the file length in the FCB
3. free the OFT entry
4. return the status of the close operation to the calling process