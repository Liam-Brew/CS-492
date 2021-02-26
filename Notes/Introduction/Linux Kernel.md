# Linux Kernel

## Table of Contents

- [Linux Kernel](#linux-kernel)
  - [Table of Contents](#table-of-contents)
  - [Kernel Source Directory tree](#kernel-source-directory-tree)
  - [System Calls](#system-calls)
    - [Adding a System Call](#adding-a-system-call)
    - [Process Management System Calls](#process-management-system-calls)
    - [File Management System Calls](#file-management-system-calls)
    - [Miscellaneous System Calls](#miscellaneous-system-calls)

## Kernel Source Directory tree

- **arch/**: architecture-dependent infrastructure
- **include/**: C libraries (e.g. stdio)
- **init/**: initialization routines
- **mm/**: memory management
- **drivers/**: device drivers
- **ipc/**: Inter-Process communication
- **fs/**: filesystem
- **kernel/**
- **net/**
- **block/**
- **lib/**
- **scripts/**
- **Documentation/**
- **samples/**
- **tools/**

## System Calls

One form of process-to-OS communication. System calls vary from one OS to another and are invoked when a running application needs a system service

### Adding a System Call

![syscall_code](/notes/assets/introduction/syscall_code.PNG)

Use the macro ```SYSCALL_DEFINE2``` to declare that it is a system call. "2" means that there are two function arguments. Return value is what the system call returns. Use ```copy_{to|from}_user to copy data to/from the process address space


### Process Management System Calls

| Call                                  | Description                                     |
| ------------------------------------- | ----------------------------------------------- |
| pid = fork()                          | create an identical child process to the parent |
| pid = waitpid(pid, &statloc, options) | wait for a child to terminate                   |
| s = execve(name, argv, environp)      | replace a process' core image                   |
| exit(status)                          | terminate process execution and return status   |

### File Management System Calls

| Call                                 | Description                                   |
| ------------------------------------ | --------------------------------------------- |
| fd = open(file, how, ...)            | open a file for reading, writing, or both     |
| s = close(fd)                        | close an open file                            |
| n = read(fd, buffer, nbytes)         | read data from a file into a buffer           |
| n = write(fd, buffer, nbytes)        | write data from a buffer into a file          |
| position = lseek(fd, offset, whence) | move the file pointer                         |
| s = stat(name, &buf)                 | get a file's status information               |
| s = mkdir(name, mode)                | create a new directory                        |
| s = rmdir(name)                      | remove an empty directory                     |
| s = link(name1, name2)               | create a new entry, name2, pointing to name 1 |
| s = unlink(name)                     | remove a directory entry                      |
| s = mount(special, name, flag)       | mount a file system                           |
| s = unmount(special)                 | un-mount a file system                        |

### Miscellaneous System Calls

| Call                     | Description                           |
| ------------------------ | ------------------------------------- |
| s = chdir(dirname)       | change the current working directory  |
| s = chmod(name, mode)    | change a file's protection bits       |
| s = kill(pid, signal)    | send a signal to a process            |
| seconds = time(&seconds) | get the elapsed time since 1 Jan 1970 |

