# Process and Process Management

- What is a process? 
- How are processes represented by the OS?
- How are multiple concurrent processes managed by the OS?

### Visual Metaphore: Process

Process is like an order of toys:
- state of execution
    - completed toys waiting to be sipped
    - in progress toys
- parts and temporary holding area
    - plastic pieces, containers, etc.
- may require special hardware
    - sewing machine
    - glue gun
    - nail gun

OS Process:
- state of execution
    - program counter
    - stack
- parts and temporary holding area
    - data
    - register states living in memory
- may require special hardware
    - I/O devices
    - control which processes that are running concurrently get access to the special hardware/what order that is queued in 

### What is a Process?

OS manages hardware on behalf of applications

Application: program on disk, flash memory, stored in the cloud
    - static entity

Process: state of a program when executing, loaded into memory
    - active entity

If the same application is launched more than once there are multiple processes

Think about multiple instances of a text editor editing different files. 
    - they have separate execution states and respective data

## Process anatomy

Types of state
- Text and data
    - static state when process first loads

- heap
    - dynamically created during execution

- stack
    - grows and shrinks
    - LIFO

### What does a Process "look like"?

- Address space
    - "In Memory" representation of a process
- Page tables
    - mapping of virtual to physical Address
- Physical address
    - locations in physical memory
    - DRAM

![virtual memory example](./images/1.png)

#### example

Page Table entry: `0x03c5 --> 0x0f0f`, data may live at `0x03c5` in the virtual address space and this may map to `0x0f0f` in physical memory.

#### SWAP Space

32bit addresses -> 4GB, because of this, the OS decides dynamically which portion of which address space are stored in physical memory.

The processes can partionally be stored in memory with pointers to the rest of the process memory stored in SWAP space on disk.

![swap space](./images/2.png)

Operating Systems maintain the information where the virtual addresses actually are, whether its in memory or on disk because, it maintains the mapping between the virtual addresses and the physical memory location for everything within the memory address space.

#### Address Space

2 Processes: P1, P2, can have the exact same virtual address space size. It is up to the Operating System to maintain the location of each piece of data per process.

The operating system maintains the mapping from a given virtual address to the physical address location.

## How does the OS Know what a Process is doing?

OS **MUST** know what a process was doing so that when its stopped, it must be able to resume
the process from the exact same point, as if nothing had happened to the process.

Each process thinks that it is being ran continuously from beginning to end of execution 
on the CPU, when in reality it is scheduled, ran, stopped constantly until it is finished.

**How does the OS know what a process is doing?**

When a program is compiled, it gets translated to bytecode/binary (as an executable) sequence of instructions. At **ANY** time the CPU has a "PC" or Program Counter to keep track of where exactly in the instruction set the program is.

There are registers on the CPU that maintain certain data about the program while its running. Registers are considered to be apart of the sate of the process. Additionall, the Process **Stack** is also apart of the program state.

OS Maintains Information about:
- Program Counter
- CPU Registers
- Stack Pointer

All of this information isn maintained in a PCB: Process Control Block.


## PCB: Process Control Block

The PCB is a data structure that the operating systems leverages and maintains throughout the lifetime of any given process. 

![Process Control Block Example](./images/3.png)

- PCB in created when a process is created.
- Certain fields are updated whenever the process state changes.
    - when a process requests more memory, the OS will allocate more meory and establish new virtual to phews coal memo mappings for this process.
    - this will reflect in the memory limits as well as the information regarding valid virtual addressing space for process.

For some data fields with the PCB data structure are updated extremely regularly, like the PC or program counter changes when each instruction is executed on the CPU. We **do NOT** want to update the PC after each instruction execution as that will create a lot of overhead/bookkeeping.

Instead, the CPU has a dedicated register which is uses to track the current PC for the current executed process. The register will get updated after each instruction is executed in the process.

### How is PCB used?

Whenever a process is done executing on a CPU (and has not finished yet), it will save the PCB for the given process. When the new process is loaded it will restore it's respective PCB onto the CPU to resume the execution.

When a process is switched from one to another, saving and restore the respective PCB's, this is called **Context Switching**

#### Why is Context Switching Expensive?

- Direct Costs: number of cycles for load and store instructions (to and from memory)
    - Number of cycles for load 2 store instructions
- Indirect costs: Process A running on CPU, a lot of data is sotred in the processor cache, when we switch, and P2 is loaded, we will see a lot of cache misses because their data is completely different. Accessing CPU cache is much faster than memory, there is a hierarcical cache, 3 levels of varying size and speed.
    - COLD Cache
    - Cache misses

Cache is hot in the middle of process, when we context switch, we will have a COLD cache, because we have to spend a lot of time to read data from memory. Running with a cold cache will require longer data acess time which will slow the process.

## Process lifetime

A process can be in 1 of 2 states: **Running** or **Idle**. 

- When a processing running, it can be interrupted by a context switch. It transitions to the ready state, meaning that its ready to be scheduled on the CPU for processing again.

- When a process if Idle/Ready, it can be scheduled. A context switch will place the Idle/Ready scheduled task back on the CPU for processing.

![Process Lifetime](./images/4.png)

- When a process spawns, the OS allocates resources, when the resources are available, the process is **admitted** and is ready to be scheduled.
- When a process is in the **ready** state, it is ready to be processed on the CPU. A ready process must wait for the OS Scheduler to schedule its time/turn on the CPU.
- **Scheduler Dispatch** transitions a ready process into the **Running** state.
    - A **running** process can be processed until a context switch occurs, where the OS will interrupt the process to carry out the context switch
        - After the process has been context switched, it will transition back into the ready state
    - Sometimes, a process may need to wait for some long running event
        - Querying data from storage
        - Waiting on I/O from keyboard
    - The process enters a **watiing** state
    - When the **waiting** is finished, (the event has occured) the process enters the **ready** state again

- When all operations in the program has finished, or when the process encoutners an error, it returns the respective exit code: success, error, and the process is terminated.

## Process Creation

Mechanisms for process Creation

- Fork
    - copies the parent PCB into a new child PCB
    - child contrinues execution at instruction (PC) after fork
    - Both the parent and child have the same PCB
- Exec
    - replace child image
    - load new program and start from first instruction
    - The child PCB will have values for the new program

**The process of creating a new process is by calling "Fork", to spawn the new process, 
then calling "Exec" to replace the PCB with the relevant program/application data required
for the program to execute**

## What is the role of the CPU Scheduler?

- There are multiple processes waiting in the ready queue.
- Which process do we choose to execute on the CPU?

CPU Scheduler is an OS component that manages how the processes use the CPU resources
    - Decides which process in the ready queue is disported to the CPU to use the resources
    - Determines how long each process will run on the CPU

**Preempt**: interrupt and save current context
**Schedule**: run scheduler to choose next process
**Dispatch**: Dispatch process and switch into its context

SCheduler makes sure that the CPU time is spent running Processes instead of running scheduling algorithms. Its important ot have efficeint design and implementations within the scheduling process and efficient data structures involved in queuing processes in the ready state.
    - how long did processes run in the past, misc. data is optional

### How long should a Process run?

*How often do we run the scheduler?* The long the schedule runs, the less time can be used to process a Process. 

If we want to understand how well teh CPU time was utilized, we need to calculate the scheduling process time, add the time it takes to determine the next process for scheduling, and then divide it by the amount of time each process ran

```
( 2 * Tp) / (2 * Tp + 2 * t_scheduled)
```

![Scheduling Efficiency Bad](./images/5.png)

![Scheduling Efficiency Good](./images/6.png)

- Tp --> ***Timesclice*** time Tp allocated to a process on the CPU (the red stuff/bars)
    - what are approprocate time sclice values?
    - metrics to choose next process to run?

## I/O

![I/O](./images/7.png)

## Inter Process Communication

***Can Processes interact?*** Yes.

An OS MUST provide mechanisms for processes to interact with one another. 

In the modern scheme, more and more applications are structured as MULTIPLE processes that interact with one another

### ICP: Inter Process Communication

- Interprocess Communication Mechanisms: **IPC** 
    - Transfer data/info between address spaces
    - Maintain protection and isolation 
    - Provide flexibility and performance

Example: Message passing  ICP:

Suppose we have P1: Web Server, and P2: Database. The OS will maintain a "Buffer" where messages can be sent between these processes-> sending and receiving messages from one another for communication. A communication channel.

- OS provides communication channel like shared buffer between processes
- Processes
    - write: send
    - read: receive
    - messages to and from channel

![IPC Example](./images/8.png)

#### Advantanges

- Managed by operating system 
    - operating system provides exact same API and system calls for reading/writing from communication channel

#### Disadvantages

- Overhead
    - every single piece of information we want ot send between processes must be 
        - copied from the user space of one of the processes
        - written into the channel/buffer which is sitting in ther Kernel memory of the OS
        - read back into the address space into the other process


### ICP: Shared Memory

- OS establishes a shared channel and maps it into each process address space
- Processes can read/write directly from this memory
- OS is **out of the way!**

#### Advantanges
- Less overhead
- OS does not get into the way of the application
- Faster communication channel between processes

#### Disadvantages
- Introduces additional complexity and possible errors within the application/processes.
- Since OS is out of the way, there are no strict/rigid/**well defined** APIs and access control mechanisms to read/write data between processes
    - more error prone
    - more complexity for developers

![Shared Memory](./images/9.png)


***It only makes sense to do shared memory based IPC if the setup cost can be amortized across a sufficiently large message set, since establishing the shared memory channel mapping is very expensive**


