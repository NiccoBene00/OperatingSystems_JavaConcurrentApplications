# OPERATING SYSTEMS
## Student: Niccolò Benedetto MAT. 7024656
## Instructor: Pierfrancesco Bellini

> [!IMPORTANT]
> In the .md file the discussion conducted during the oral exam is reported.
> The .zip file contains the concurrent JAVA application presented for the written exam.

**1) What does it mean for an operating system to be interrupt driven?**

Interrupt-driven operating systems are characterized by systematically handling system interrupts (due to user requests, peripheral requests, timers, exceptions). Unlike operating systems that use polling, an interrupt-driven OS is designed to quickly respond to both external and internal events via a mechanism called an "interrupt." Interrupts are signals sent to the processor indicating that an event requiring immediate attention has occurred. This approach allows the operating system to manage resources efficiently and respond promptly to urgent situations. There is an interrupt table that contains the addresses of the service routines associated with each interrupt.

**2) What are synchronous interrupts and how are they handled?**

Synchronous interrupts are events that occur in a correlated and synchronized manner with the program execution. This means they happen at specific points in the flow and are directly linked to the operations in progress. The three most common forms of synchronous interrupts are:
  - Infinite loops;
  - Accesses to invalid memory areas;
  - Concurrent accesses to the same memory area (for example, when some processes try to write to a particular memory address that is simultaneously being read by other processes).

The latter two cases are handled by introducing a dual-mode approach in the operating system: user mode and kernel mode. While user mode is accessible by the user for basic functions, kernel mode is directly accessible by the OS and allows communication with all the system’s hardware resources. The first case is instead resolved by introducing a special timer that “wakes” the system at regular intervals for a check: if an infinite loop is detected, an attempt is made to break out of it; otherwise, the timer is reset and the system continues executing instructions.

**3) Introduce system calls**

System calls are requests made by processes to gain authorization to perform operations typical of kernel mode (for example, memory allocation). Once this authorization is obtained, the process can execute the instructions associated with that system call. Each type of system call and its associated instructions are contained in a table. A process making a system call might need to pass some parameters. This parameter passing occurs in three different ways:
  - Through the CPU registers;
  - Through the memory address where the parameter is stored;
  - Through the STACK.

**4) Introduce the concept of a virtual machine with particular reference to the JVM**

A virtual machine is an abstraction that allows one to obtain a sort of “virtual computer” inside a physical computer. This technique offers the possibility of running multiple operating systems on the same hardware. Naturally, the more virtual machines installed on the same system, the more the CPU time will be alternated, since there is a limited number of processors on each computer. However, each virtual machine perceives itself as having its own kernel and being completely separated from the others, thereby providing an isolated and autonomous environment for the execution of processes and applications. The Java Virtual Machine (JVM) uniquely allows JAVA programs to run on any physical machine. In fact, the JVM is responsible for executing the bytecode, which is the intermediate language between the source code written by the programmer and machine language, generated as a result of compiling the source file. The main characteristics of the JVM are:
  - **Platform independence:** once compiled, the bytecode can be executed on any platform that has a JVM without requiring recompilation;
  - **Garbage Collector:** a mechanism is directly implemented to manage memory allocation and deallocation (preventing memory leaks);
  - **Security:** built-in security mechanisms check bytecode instructions;
  - **Exception handling;**
  - **JIT Compilation:** Just-In-Time compilation allows the bytecode to be compiled into native machine code during program execution;
  - **Multithreading support:** a module that enables the creation and management of threads within the same JAVA program, thus supporting concurrent and parallel applications.

**5) List the possible states a process can be in from its creation until its termination**

During its lifecycle, a process can be in five different states:
  - **"new":** the process has been created, but has not yet executed its first instruction. It still needs to obtain the necessary resources.
  - **"ready":** the process has acquired all necessary resources except the CPU.
  - **"running":** the process is currently executing on the CPU.
  - **"waiting":** the process is waiting for some type of event (e.g., I/O) to continue its execution.
  - **"terminated":** the process has finished executing all instructions and the allocated resources are released.

The possible transitions among these states are:
  - from "new" to "ready";
  - from "ready" to "running";
  - from "running" to "terminated";
  - from "running" to "waiting";
  - from "waiting" to "ready"; 
  - from "running" to "ready".

**6) Introduce the Process Control Block**

The PCB is a data structure that contains the main information regarding an executing process. It includes the PID (process identifier), the current state of the process, the value of the Program Counter (the register that holds the address of the current instruction), the state of the CPU registers, the number of clock cycles executed by the process up to that moment, and the files used and accessed during execution.

**7) Provide an overview of the queues necessary for process scheduling**

Modern operating systems use three different queues for managing process scheduling:
  - **Job queue:** a queue that contains all the processes in the system (those in the ready, running, or waiting states);
  - **Ready queue:** a queue that contains only the processes in the ready state, i.e., those waiting to be assigned to the CPU for execution;
  - **Device queue:** a queue that contains processes waiting for an I/O event or other external events.

Process scheduling can be of two types:
  - **Long-term scheduling (job scheduling):** the OS is responsible for moving processes from the waiting queue to the ready queue, i.e., scheduling processes that are in secondary memory and need to be moved to primary memory.
  - **Short-term scheduling (CPU scheduling):** the OS assigns processes from the ready queue to the CPU based on a specific scheduling policy.

**8) What is a context switch?**

A context switch represents the operation carried out by the processor when switching from one process to another. During a context switch, the CPU continues to work but performs operations to save registers (saving the state of the process being switched out and loading the state of the new process) rather than performing useful work.

**9) What is a zombie process?**

A process becomes a zombie when it is a child process that has terminated without the parent process retrieving, for example, its return value. It is called a zombie process because, despite having finished executing all associated instructions, its PCB is still allocated, thus unnecessarily consuming system resources.

**10) How does communication between processes occur in an operating system?**

Inter-process communication (IPC) in an operating system can occur in two different ways:
  - **Shared memory:** a common area is created that is accessible by multiple processes needing to access the same information. This method is used when exchanging large amounts of data, even among more than two processes.
  - **Message passing:** processes exchange messages via the kernel. This method is used when only small amounts of data are to be exchanged for efficiency or when synchronizing different processes.

**11) Elaborate on inter-process communication via message passing**

Message passing involves two operations, *send(message)* and *receive(message)*, which can be executed in two ways:
  - **Direct communication:** the PID of both the sender and receiver must be known, so the operations become *send(P, message)* and *receive(P, message)*. This mode allows communication between only two processes at a time and can be implemented unidirectionally or bidirectionally.
  - **Indirect communication:** in this case, the operating system opens a communication port identified by an ID. All processes that know the port's ID can use it via *send(IDPort, message)* and *receive(IDPort, message)*. Note that when the port is shared among several processes, it is also necessary to determine how to handle message reception.

**12) Explain the potential of a multithreaded system**

In a single-threaded system, the operating system executes the instructions of processes as if there were only one sequence of instructions, not exploiting the true power of today’s processors. With multithreading, the same sequence of instructions can be executed in parallel (referred to as fake parallelism in uniprocessor systems and true parallelism in multiprocessor systems).  
Threads can be classified as user threads and kernel threads: the former perform basic operations, while the latter handle kernel-level operations.  
The relationship between these two types of threads can be implemented in three different configurations:
  - **Many-to-one:** multiple user threads are managed by a single kernel thread. This mode is suitable for uniprocessor systems. However, if a user thread makes a blocking call, the kernel thread must block all the other user threads.
  - **One-to-one:** each user thread is managed by its own kernel thread. A blocking call in one thread does not interfere with the others, but this mode offers a limited degree of multiprogramming based on the number of processors in the system.
  - **Many-to-many:** N user threads are managed by M kernel threads. This configuration, when applicable, allows optimal utilization of the processor’s resources.

**13) Detail Linux scheduling**

In the UNIX world, each process is assigned two types of priority: static and dynamic. The static priority is represented by the *nice value*, a fixed number that determines the overall priority of the process, while the dynamic priority can vary over time and is either manually set by the user or assigned by the kernel.  
CPU scheduling is based on the concept of an epoch: before each epoch, the set of processes to be executed is selected and, for each process, the time quanta to be assigned are determined. Once a process has used up its time quanta, it is put on hold until the next epoch. The selection of time slices is based on a "goodness" function, which returns a score for the process based on the ratio between its CPU usage and its *nice value*.  

With the Linux O(1) Scheduler, process scheduling is performed in constant time regardless of the number of ready processes at that moment. Each process is assigned a priority level ranging from 0 (highest priority) to 139 (lowest priority). There are two FIFO (First-In-First-Out) queues for each priority level: one, called the "active queue," that holds processes that have not yet finished their time quanta, and the other, called the "expired queue," which contains processes that have used up their time quanta. The scheduler simply selects the highest priority process from the active queue and assigns the CPU to it. When all active queues are empty, a swap is made: the expired queues become the active queues and vice versa. To favor those processes that, due to their priority, are scheduled for high CPU usage but are mostly in I/O (for example, preempted by higher-priority processes), a bonus mechanism is implemented—i.e., an increase in priority. Generally, this bonus varies within the range [-5, +5].

In recent versions of Linux, the "Linux Completely Fair Scheduler" was introduced—a new scheduler that dynamically assigns the CPU to processes using a red-black tree structure to ensure efficient updates and O(log2 n) search performance. The priority level is updated based on the *nice value* and the CPU usage time.

**14) Explain the priority inversion problem**

This scenario occurs when a low-priority process (L = low) holds a resource R that is requested by a higher-priority process (H = high). Priority inversion occurs when a third process with a priority lower than H but higher than L (M = medium) preempts L, even though it does not urgently need resource R. In this case, the execution order of the processes would be M-L-H. One solution to this scenario is to temporarily elevate the priority of L—i.e., raise L's priority at least to that of H.

**15) What is meant by deadlock (or stalemate) among a set of processes? How does an operating system handle it?**

Deadlock occurs when a set of processes reaches a state where one process requests a resource already allocated to another process, which in turn is requesting a resource held by the first process. As a result, both processes wait indefinitely because the system cannot satisfy their requests.  
Operating systems handle deadlock using three different techniques:
  - **Prevention:** aims to ensure that the described situation never occurs. This can be achieved by enforcing that when a process requests a resource, it does not already hold one, or by requiring it to release all held resources when making a new request.
  - **Detection and recovery:** the operating system periodically checks whether processes are not making progress in satisfying their requests and then applies a mechanism to recover the total state of the processes (rollback) before the deadlock occurs. This method requires sophisticated state management and may introduce execution delays.
  - **Ignoring the problem:** many modern operating systems simply overlook the deadlock issue, delegating its resolution to the affected applications.

**16) How is contiguous memory allocation implemented?**

Assuming that the operating system assigns each process a sufficiently large portion of memory to contain it, a process is defined by a memory interval (base + limit). When the process requests access to memory, the MMU (Memory Management Unit) adds the requested offset to the process’s base address; if the limit is not exceeded, the access is granted, otherwise it is denied.  
When the process terminates, the memory it occupied is freed, creating a "hole"—that is, free memory that can be allocated to new processes. Contiguous memory allocation is based on three different fitting criteria:
  - **First fit:** allocate the first hole that is large enough to hold the process.
  - **Best fit:** allocate the smallest hole that is sufficient for the process.
  - **Worst fit:** allocate the largest hole for the new process.

While best fit is the most efficient approach, it requires more time and computational resources. Additionally, it causes fragmentation. **External fragmentation** occurs when many small free memory portions (holes) are created, none of which can individually accommodate the new process (even if their total sum might be sufficient). A possible solution is compaction: the operating system rearranges the free memory areas to form a single contiguous block—a costly and time-consuming operation. **Internal fragmentation** occurs when a process uses only a small part of its allocated memory area.

**17) Discuss paging**

In operating systems that manage memory using paging, logical addresses (addresses generated by the CPU that allow processes to use a continuous and isolated addressing space, independent of the actual physical memory layout) are divided into pages, while physical addresses (the result of the MMU mapping of a logical address) are divided into frames. Pages and frames have the same size (usually a power of two), but the first page does not necessarily correspond to the first frame; they can be flexibly assigned based on memory needs. To manage the mapping between pages and frames, each process is assigned a page table that maps pages to the corresponding frames. Additionally, each page is accompanied by a validity bit that indicates whether the page has been used.  
Paging uses three different approaches to efficiently manage memory utilization:
  - **Hierarchical paging:** divides the page table structure into multiple hierarchical levels, ensuring efficiency even for large address spaces.
  - **Paging with hash tables:** the mapping between pages and frames in the page table is implemented using a hash table. This approach is generally considered when addresses are larger than 32 bits, i.e., when the page table becomes prohibitively large.
  - **Inverted paging:** uses a single page table instead of one per process. In this case, the logical address consists of a process identifier, the page number, and the offset. This method is efficient in terms of memory usage but makes sharing pages between processes more difficult.

**18) Provide a comprehensive overview of the concept of virtual memory**

Virtual memory refers to the technique used by the operating system to simulate an increase in primary memory relative to the physical machine. Although the RAM may be free at startup, as processes accumulate (especially in machines with limited RAM), memory can fill up quickly, leading to memory errors. Modern operating systems implement virtual memory through demand paging. This policy defines when and how to bring a page stored on disk (secondary memory) into RAM (primary memory). Each time this operation is performed, the validity bit of the incoming page must be checked: if it is set to 1, the page is already present. Instead, the page to be replaced is chosen according to one of three criteria:
  - **FIFO:** the oldest page loaded into RAM is the first to be replaced to make room for the new page. This policy is not optimal as it introduces Belady’s anomaly—in some sequences, increasing the number of frames increases the number of RAM accesses, resulting in a higher page fault rate.
  - **Optimal replacement:** the page that will be used furthest in the future is removed. However, this policy requires the operating system to have knowledge of each process’s future behavior, making it not always achievable.
  - **LRU (Least Recently Used):** the page that has not been used for the longest time is removed. Typically, this technique involves associating a stack or counter with each process. However, it is computationally expensive, so modern operating systems use a simplified version known as pseudo-LRU: each time a page is used, a validity bit is set, and pages with a zero bit are replaced.

**19) What is thrashing?**

Thrashing is a problem associated with demand paging. It can occur when the CPU is predominantly occupied with continuously transferring pages between secondary and primary memory rather than executing processes. This situation leads to a drop in CPU utilization, which the processor might interpret as an indication to terminate some processes, thereby causing more processes to enter the ready queue. One solution would be to load the entire working set of each process into memory. The working set is defined as the set of all pages that a process actively uses throughout its lifetime. Although this is an excellent solution, it is not always feasible since some processes have very large working sets.

**20) What is the DMAC hardware component?**

DMAC (Direct Memory Access Controller) refers to a hardware component that allows system peripherals to access memory without the direct involvement of the processor, thereby freeing the CPU for other priority tasks while enabling background data transfers.

