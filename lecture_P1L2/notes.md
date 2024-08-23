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





