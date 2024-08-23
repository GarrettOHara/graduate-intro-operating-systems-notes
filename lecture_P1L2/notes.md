# Introduction to Operating Systems

## What is an Operating System?

Software that ***Abstracts*** and ***Arbitrates*** the computer hardware.

## Visual Metaphor - Toy Shop Manager


### Directs operational resources
Toy Shop

----
- Control employee time
- Control parts, tools

Operating System

---
- Control use of CPU, Memory, Peripheral devices


### Enforces working policies
Toy Shop

---
- Ensure fairness policies, safety, clean up
- Manage workers sharing resources

Operating System

---
- Fair resource access
- Number of open files that can be open per process
- Thresholds that can limit before memory management daemons kick in

### Mitigates difficulty of complex tasks
Toy Shop

---
- Simplifies operations, optimize and simplify operations
- Optimize performance

Operating Systems

---
- Abstract hardware details
- Interact with operating system via system calls

## What is an Operating System

- CPU/Processor: Modern CPUS contain more than 1 processing units, mulit core.
- GPU: graphics Processor
- RAM: main memory
- USB: allow IO
- Ethernet Wifi Card/NIC
- Disk: Storage

All of these hardware components are used by applications. Multiple applications can share these hardware devices.

---

*The Operating System is a layer of system software that sits between complex hardware and the applications/application developers.*

- **Hide hardware complexity**
  - When developing an application, you don't want to think of the bits and package that are being send/received via Ethernet or via WiFi, or both!
  - The operating system abstracts the networking into send/receive data that is delivered via ports binded to sockets

*Operating Systems also manage hardware in some cases. When running an application the Operating System manages the amount of memory that is allocated to each application.*

- **Provide resource management**
  - Schedules the processes on the CPU
  - Allocates amount of memory provisioned to each process. Can dynamically grow memory of processes if requirements change.
- **Provide isolation and protection**
  - Allocates different physical parts of the memory and ensure that application cannot access memory alocated to other processes/applications.

  ---

  ### Operating System Summary Definition

  *An Operating System is a layer of system software that:*
  - *Directly has privileged access to the underlying hardware*
  - *Hides the hardware complexity*
  - *Manages hardware on behalf of one or more application according to some predefined policies*
  - *Ensures that applications are isolated and protected from one another (reading data when not permitted, overwriting memory addresses/data)*

# Quiz 1

Which of the following are likely components of an operating system? Check all that apply
- file editor
- file system
- device driver
- cache memory
- web browser
- scheduler

---

### Answer


- file system
- device driver
- scheduler

# Quiz 2

For the following options, indicate if they are examples of abstractions(B) or arbitration (R)

distributing memory between multiple processes ___

supporting different types of speakers ___

interchangeable access of hard disk or SSD ___


### Answer

- R
- A
- A

# Operating System Examples

- Mainframes / High end servers
- Desktop
  - Windows
  - Unix-based
    - Mac (Extends the Unix BDS [Berkley System Distribution of Unix])
    - Linux
      - Ubuntu, Arch btw, etc.
- Embedded
  - Smartphones / Tablets
    - Android
    - iOS
    - Symbian

*There are a number of unique choices between these operating systems. we will focus on Linux*

# OS Elements

### Abstractions

- Process, thread
- File, socket, and memory page
  - more closely relates to the underlying hardware they represent


### Mechanisms

- Create, schedule, open, write, allocate

### Policies

- Least Recently Used (LRU)
  - Typically for cache management
- Earliest Deadline First (EDF)

*Determine which process/data  to evict from memory*

## OS Elements: Memory Management Example

- Abstractions 
  - Memory page
- Mechanism
  - Allocate, map to a process
- Policies
  - Least recently used LRU to determine weather the contents of a memory page is stored in memory or on disk Storage 
  - When we change from physical memory to disk storage this is known as "Swapping"

## Design Principles

Separation of mechanism and policy
- implementation flexible mechanisms to support many policies
  - LRU, LFU, random

Optimize for common cases
  - Where will the OS be used (what type of machine/environment)?
  - What will the end user be doing on the machine?
  - What are the workload requirements?

## Basic OS Services

- scheduler 
  - controls access to the CPU(s)
- memory Manager
  - controls access to memory
  - ensure applications don't overwrite other application's memory
- block device driver 
  - responsible for block device like disk

*higher level abstractions*
- file system
  - files used by almost all Operating Systems

*Operating Systems need to provide certain functionalities for clients or application developers to be useful*
- Process management
- File management
- Device management


Operating systems makes all of these functionalities available via ***System Calls***

### Windows vs Linux System Calls

Very similar types of system calls but are different names API endpoints.

# Quiz 3

On a Linux 64 Bit Operating System which System Call is used to:

- Send a signal to a process
- set the group identity of a process
- mount a file system
- read/write system parameters

### Answer

- Kill
- SETGID
- MOUNT
- SYSCTL

# Operating System Approaches

## Monolithic OS

*Historically Monolithic OS were popular/primary OS design.*

When every possible service (memory management, device drivers, file management, process / thread, scheduling) that any one of the applications may require or that any type of hardware will require/demand is already apart of the operating system.


***Downsides:***
  - This can potentially make the Operating System really large 
  - A lot of overhead
  - Customization can be lacking or hard to implement
  - Large memory footprint
  - Sometimes poor performance
***Benefit:***
  - Everything is included
  - Inclining, compiling optimizations


## Modular OS

Has a number of basic services and APIs apart of it already. But everything that is required can be added as a module
  - We can choose what type of module we want to customize the service for the best use case
    - choose what file system you want, device block driver to manage drives, memory manager, scheduler, etc.
    - Depending on the workload, we can install a module to implement the module's interface we can install a file system optimized for random access, or if there is other workloads on the computer we can do it for sequential access

***Benefits:***
- Easier to maintain and upgrade
- Smaller code base less overheard
- less resource needs
  - can leave more memory for the actual application running on the instance
  - can lead to better performance as well

***Downsides:***
- Indirection can impact performance
- Maintenance can still be an issue because we depend on 3rd parties to maintain the modules and caon be poorly maintained or contain bugs

Overall, more common today.

## Micro kernel

- Only require the most basic primitives at the operating system level
- can support some basic services to represent
  - address space, context, and thread

Typical services we usually think of as apart of the operating system like
  - file system
  - device drivers
  - applications like databases

All typically run outside of the operating system at User Level

Requires lots of interprocess iterations

Will support inter process communication as one of lots core services within the OS as well as address spaces and threads

***Benefits:***
- Very small in size/footprint
- Lower overhead and better performance
- Verifiability
  - Since the footprint and overhead are low, it is easier to verify that software behaves exactly as it should
  - Great test environment for QA of software
  - Makes micro kernels valuable for in environments where the operating system stability os very important like embedded systems

***Drawbacks:***
- Portability is questionable because each kernel is specialized for the underlying hardware it is running on
  - hard to find common components for software
- Significant amount of interprocess communication that comes at a cost of user/kernel level crossing
- Complexity of software development
  - each environment is different. hard to make software operate in all micro kernel environments.
