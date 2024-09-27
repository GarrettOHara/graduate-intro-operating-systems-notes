# P2L4: Thread Design Considerations

# Kernel vs User Level Threads

- OS Kernel mtaintains
    - thread abstraction
    - scheduling
    - syncing

- User level library provides
    - thread abstrction
    - scheduling sync
    - Different processes can use different thread libraries that may provide different memory management, cpu scheduling/etc.

- Mappings allow
    - 1:1, M:1, M:M thread mapping

- Virtual address mapping
- Stack
- Registers

# Thread Data Structures

## Single CPU

- When a process' entire state exists in a single PCB data structure
- The thread can trap into a kernel level thread.

### Mulithreaded

*Many to 1 structure*

- Many user level threads, user level thread library representing it
    - needs to track resource use to make scheduling decisions and resource provision
- 1 Kernel level thread


