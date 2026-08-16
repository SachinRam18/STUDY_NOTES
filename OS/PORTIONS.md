# OPERATING SYSTEMS — FULL PORTIONS

## UNIT 1 — INTRODUCTION TO OPERATING SYSTEMS

### 1. Operating System Basics

- Definition of Operating System
- Need for Operating System
- Goals of Operating System
- Functions of Operating System
- Evolution of Operating Systems
- Types of Operating Systems

- Batch OS
- Multiprogramming OS
- Multitasking OS
- Time-sharing OS
- Distributed OS
- Network OS
- Real-time OS
- Embedded OS
- Mobile OS

### 2. Computer System Organization

- Computer system components
- CPU
- Main memory
- I/O devices
- Storage devices
- System bus
- Interrupts
- DMA

### 3. Operating System Structure

- OS components
- Kernel
- User mode
- Kernel mode
- Dual-mode operation
- OS architecture
- Monolithic structure
- Layered structure
- Microkernel
- Modular structure
- Hybrid systems

### 4. Operating System Services

- Program execution
- I/O operations
- File-system manipulation
- Communication
- Error detection
- Resource allocation
- Accounting
- Protection and security

### 5. User–OS Interface

- Command Line Interface
- GUI
- Shell
- System programs

### 6. System Calls

- System call concept
- System call interface
- Types of system calls

- Process control
- File management
- Device management
- Information maintenance
- Communication
- Protection

### 7. System Boot

- Boot process
- BIOS/UEFI
- Bootloader
- Kernel loading
- OS initialization

### 8. Virtualization

- Virtual machines
- Hypervisors
- Type 1 hypervisor
- Type 2 hypervisor
- Virtualization in OS

---

# UNIT 2 — PROCESS MANAGEMENT

## 1. Process Concept

- Program vs Process
- Process states
- Process state diagram
- Process Control Block — PCB
- Process creation
- Process termination

## 2. Process Operations

- Parent and child processes
- Process creation
- `fork()`
- `exec()`
- `wait()`
- `exit()`
- Zombie process
- Orphan process

## 3. Process Scheduling

- Scheduling concept
- Scheduler
- Long-term scheduler
- Short-term scheduler
- Medium-term scheduler
- Dispatcher
- Context switching

## 4. Scheduling Criteria

- CPU utilization
- Throughput
- Turnaround time
- Waiting time
- Response time

## 5. CPU Scheduling Algorithms

- FCFS
- SJF
- SRTF
- Priority Scheduling
- Round Robin
- Multilevel Queue
- Multilevel Feedback Queue

### Numerical Problems

- Gantt chart
- Completion Time
- Turnaround Time
- Waiting Time
- Response Time
- Average waiting time
- Average turnaround time

## 6. Threads

- Thread concept
- Process vs Thread
- Benefits of threads
- User-level threads
- Kernel-level threads
- Multithreading
- Multithreading models

- Many-to-One
- One-to-One
- Many-to-Many

## 7. Inter-Process Communication

- IPC
- Shared memory
- Message passing
- Pipes
- Sockets
- Signals

---

# UNIT 3 — PROCESS SYNCHRONIZATION AND DEADLOCKS

## 1. Process Synchronization

- Race condition
- Critical section
- Critical-section problem
- Requirements of critical section

## 2. Synchronization Solutions

- Peterson's solution
- Hardware synchronization
- Atomic operations
- Test-and-set
- Compare-and-swap

## 3. Semaphores

- Semaphore concept
- Binary semaphore
- Counting semaphore
- `wait()`
- `signal()`

## 4. Classical Synchronization Problems

- Producer–Consumer problem
- Readers–Writers problem
- Dining Philosophers problem

## 5. Monitors

- Monitor concept
- Condition variables
- Monitor-based synchronization

## 6. Deadlocks

- Deadlock concept
- Necessary conditions

- Mutual exclusion
- Hold and wait
- No preemption
- Circular wait

## 7. Deadlock Handling

- Deadlock prevention
- Deadlock avoidance
- Deadlock detection
- Deadlock recovery

## 8. Deadlock Algorithms

- Resource Allocation Graph
- Banker's Algorithm
- Safety algorithm
- Resource-request algorithm
- Deadlock detection algorithm

### Important Numericals

- Banker's Algorithm
- Safe sequence
- Resource allocation
- Deadlock detection

---

# UNIT 4 — MEMORY MANAGEMENT

## 1. Memory Management Basics

- Memory hierarchy
- Address binding
- Logical address
- Physical address
- MMU
- Dynamic loading
- Dynamic linking

## 2. Contiguous Memory Allocation

- Single partition
- Multiple partitions
- Fixed partitioning
- Variable partitioning

## 3. Allocation Techniques

- First Fit
- Best Fit
- Worst Fit
- Next Fit

## 4. Fragmentation

- Internal fragmentation
- External fragmentation
- Compaction

## 5. Swapping

- Swapping concept
- Swap space
- Swapping techniques

## 6. Paging

- Paging concept
- Pages
- Frames
- Page table
- Address translation
- TLB
- Multilevel paging
- Inverted page table

## 7. Segmentation

- Segmentation concept
- Segment table
- Logical address
- Segmentation with paging

## 8. Virtual Memory

- Virtual memory concept
- Demand paging
- Page fault
- Page-fault handling
- Copy-on-write

## 9. Page Replacement Algorithms

- FIFO
- Optimal
- LRU
- Second Chance
- Clock algorithm

### Important Numericals

- Page-table address calculation
- Page replacement
- Page faults
- FIFO
- Optimal
- LRU
- TLB
- Effective access time

## 10. Thrashing

- Thrashing concept
- Causes
- Working-set model
- Page-fault frequency
- Thrashing control

---

# UNIT 5 — FILE SYSTEM AND I/O MANAGEMENT

## 1. File System

- File concept
- File attributes
- File operations
- File types
- File access methods

- Sequential
- Direct
- Indexed

## 2. Directory Structure

- Single-level directory
- Two-level directory
- Tree-structured directory
- Acyclic graph directory
- General graph directory

## 3. File-System Implementation

- File-system structure
- File-system mounting
- File control block
- In-memory file system

## 4. File Allocation Methods

- Contiguous allocation
- Linked allocation
- Indexed allocation

## 5. Free-Space Management

- Bit vector
- Linked list
- Grouping
- Counting

## 6. File Protection

- Access control
- Access Control List — ACL
- Protection mechanisms

## 7. I/O Systems

- I/O hardware
- I/O controller
- Device drivers
- Interrupt-driven I/O
- DMA
- Buffering
- Caching
- Spooling

## 8. Disk Management

- Disk structure
- Disk scheduling
- Disk formatting
- Boot block
- Bad blocks

## 9. Disk Scheduling Algorithms

- FCFS
- SSTF
- SCAN
- C-SCAN
- LOOK
- C-LOOK

### Important Numericals

- Disk head movement
- FCFS
- SSTF
- SCAN
- C-SCAN
- LOOK
- C-LOOK

---

# UNIT 6 — CASE STUDY / MODERN OS

Depending on the Anna University regulation, this portion can appear as a separate unit or be incorporated into the other units.

## Linux

- Linux history
- Linux architecture
- Linux kernel
- Kernel modules
- Process management
- Linux scheduling
- Linux memory management
- Linux file system
- Linux I/O
- Linux IPC
- Linux networking
- Linux security

## Windows

- Windows architecture
- System components
- Process management
- Memory management
- File system
- Networking
- Security

## Modern OS Concepts

- Android OS
- Mobile operating systems
- Virtual machines
- Containers
- Cloud operating systems
