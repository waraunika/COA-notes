# 8.1 Characteristics of multiprocessors
**Core concept**
- A multiprocessor is a single computer system that contains two or more independent processing units (CPUs or IOPs) interconnected with memory and I/O equipment.
- These processors work under the control of a single, unified operating system to solve a problem.
- Key classification: They are classified as MIMD (Multiple Instruction Stream, Multiple Data Stream) systems,
	- meaning, each processor can execute a different instruction stream of a different data stream simultaneously
## Characteristics:
- Multiple Processors: Two or more CPUs (or IOPs) reside within a single computer system
- Shared Memory & I/O: All processors share access to the main memory, I/O devices and the operating system. This creates a tightly-coupled system.
- Single OS Control: The entire system is managed by one operating system, which enables interaction and cooperation between all processors and system components.
- Improved Performance: Allows for the execution of multiple independent jobs in parallel or the splitting of a single job into parallel tasks
- enhanced reliability: If one processor fails, the system can often continue to operate at a reduced capacity, a property known as graceful degradation
## Boosting Performance/Reliability
**How performance is increased**:
- Job Parallelism: Run multiple independent applications (e.g., a web browser and a video renderer) simultaneously on different processors.
- Task Parallelism: Decompose a single, large program (e.g., a scientific simulation) into smaller subtasks that can be executed in parallel across multiple processors, significantly speeding up the solution
	- Methods: this can be done by the programmer explicitly defining parallel tasks, or by a smart compiler that automatically detects and exploits parallelism in the code
**How Reliability is increased**:
- Graceful Degradation:
	- The system is inherently more reliable because the failure of single processor does not cause a total system crash
	- The remaining processors can take over the workload
		- allowing the system to continue functioning, albeit more slowly
	- key feature for critical systems
## Multi: processor vs computer vs processing
- Multiprocessor (Tightly Coupled):
	- concept: a single system with multiple processors sharing a common central memory
	- communication: processors communicate by reading from and writing to shared memory.
	- very fast
- Multicomputer (Loosely Coupled):
	- concept: a network of independent, complete computers, each with its own private memory
	- Communication: Processors communicate by passing explicit messages over a network like LAN
	- is slower than shared memory
- Multiprocessing (The action)
	- the process or capability of a system to execute code on more than one processor.
	- a multiprocessor system performs multiprcoessing
## Classification by Memory Organisation
1. Shared Memory Multiprocessor
	- Concept: all processors share a single, unified address space. any processor can read/write to any memory location
	- analogy: a shared whiteboard in a meeting room, everyone can see and write on the same board
	- adv: fast and simple communication between processors (just write to memory)
	- disadv: memory access can be a bottleneck if many processors try to access it simultaneously, aka memory contention
	- use case: ideal for tasks that require a high degree of interaction and data sharing between parallel tasks
2. Distributed Memory Multiprocessor
	- concept: each processor has its own private local memory. no global, shared memory
	- analogy: each person in a class, has their own notebook. to share information, they must explicitly send a message
	- adv: avoids memory contention, scales more easily to a a large number of processors
	- disadv: communication between processors is slower and more complex as it requires message passing
	- use case: most efficient for problems that can be split into tasks with minimal interaction
# 8.2 Interconnection Structures
- the components that form a multiprocessor system are CPUs, IOPs connected to input-output devices and a memory unit
- the interconnection structure is the nervous system that allows processors, memory and I/O to communicate
- the interconnection between the components can have different physical configurations, depending on the number of transfer paths that are available.
	- between the processors and memory in a shared memory system
	- among the processing elements in a loosely coupled system
- there are various physical forms available for establishing an interconnection network
	- timeshared common bus
	- multiport memory
	- crossbar switch
	- multistage switching network
	- hypercube system
## Time Shared Common Bus
- concept: 
	- simplest structure
	- all components (CPUs, Memory, IOPs) are connected to a single, common set of wires (the bus).
- analogy: a single-lane road that everyone must use. one car can use the road at a time
- working:
	- a processor wanting to communicate must first check if the bus is free
	- it then places the address and data on the bus
	- only one such transfer can occur at any given time
- adv: simple low cost, easy to expand
- disadv: 
	- major bottleneck as single path limits the total system bandwidth, 
	- single point of failure as if the bus fails, the entire system fails
	- low rate of communication as only one component can communicate with other component
- evolution: to reduce bottleneck, a dual-bus structure can be used, where each processor has a local bus and connects to the system bus, via a bridge
	- this reduces traffic on the main bus
- single bus system figure:
  ![[time_shared_single_bus.png]]
- a more economical implementation of a dual bus structure is depicted in the figure below:
  ![[time_shared_local_and_system_bus.png]]
## Multiport Memory
- concept: a dedicated path is established between each processor and each memory module
	- the memory controller has multiple ports
- analogy: a switchboard with a direct, dedicated line to every office
- working:
	- the memory module itself has internal control logic
	- if two processors request the same memory module simultaneously, the controller arbitrates (e.g., using fixed priorities) to grant one access.
- adv: very high potential bandwidth due to multiple simultaneous paths
- disadv:
	- extremely complex and expensive memory controller
	- physically bulky due to the large number of cables and connections
	- difficult to scale; adding a new processor requires adding a port to every memory module
- Figure for the memory organisation:
  ![[multiport_memory_org.png]]
## Crossbar Switch
- concept: a grid of switches that creates a dedicated, non-blocking path between any processor and any memory module
- analogy: a full telephone switchboard where any incoming line can be connected to any outgoing line without blocking others (as long as the destination is free).
- working:
	- at every intersection of a processor bus and a memory path, there is a crosspoint switch
	- when a processor wants to access a memory module, the corresponding crosspoint switch is closed, establishing a direct connection
	- key feature: it supports simultaneous transfers from all processors to all memory modules as long as each memory module is accessed by only one processor at a time.
- adv: very high bandwidth, non-blocking for distinct memory modules.
- disadv: the number of crosspoints scales as m x n, making it complex and expensive for large systems (e.g., 64x64 requires 4096 switches)
- Figure for the functional design switch:
  ![[crossbar_switch.png]]
	- the small square in each crosspoint is a switch that determines that path from a processor to a memory module
- Block Diagram for the crossbar switch:
  ![[crossbar_block.png]]
## Multistage Switching Network
- concept: 
	- a compromise between the simple bus and the complex crossbar.
	- it uses multiple stages of small (e.g., 2x2) interchange switches to connect many inputs to many outputs
- analogy: a multi-level road interchange with on and off-ramps, which is more complex than a single road, but cheaper than direct road between every origin and destination
- working:
	- building block: a 2x2 interchange switch that can route its two inputs to its two outputs
	- these small switches are arranged in multiple stages (e.g, Omega Network).
	- the path is set up by configuring each stage switch based on bits of the destination address.
- adv: more scalable and lower cost than a full crossbar for a large number of processors
- disadv:
	- blocking network: unlike the crossbar, certain communication patterns can cause conflicts (blocking) even if the source and dstination are free, because an intermediate switch might be busy
	- adds some latency due to the multiple stages a message must pass through
- in a tightly coupled multiprocessor system, the processor and the destination is a memory module
- Set up the path -> transfer the address into memory -> transfer the data
- in a loosely coupled multiprocessor system, both the source and destination are processing elements.
- concept of the switch is shown below:
  ![[multistage_switching_block.png]]
- Using the 2x2 switch as a building block, it is possible to build a multistage network to control the communication between a number of sources and destinations
	- to see how this is done, consider the binary tree in the figure:
	  ![[tree_101.png]]
	- certain request patterns cannot be satisfied simultaneously. i.e. if P1 -> 000 ~ 011, then P2 -> 100 ~ 111
	- One such topology is the omega switching network, shown below:
	  ![[omega_switching_network.png]]
	- certain request patterns cannot be connected simultaneously i.e. any two sources cannot be connected simultaneously to destination 000 and 0001
## Hypercube System
- concept: 
	- a topology for distributed-memory (loosely-coupled) systems
	- processors are arranged in an n-dimensional cube
	- each processor is directly connected to its n neighbours
- working:
	- system has N = 2$^n$ processors
	- each processor is assigned a unique n-bit binary address
	- a direct connection exists between processors if their binary addresses differ by exactly one bit
	- example: in a 3-cube, processor 000 (0) is connected to 001 (1), 010 (2) and 100 (4)
- example:
	- n = 1: 2 processors (0 and 1), connected by a line
	- n = 2: 4 processors (00, 01, 10, 11) arranged in a square. 
		- each corner is connected to 2 others
	- n = 3: 8 processors arranged in a cube
		- each corner is connected to three others
- Routing: to send a message from a source to a destination:
	- compute the exclusive-OR (XOR) of the source and destination address
	- the result is a bit mask indicating which bits differ
	- the message is routed through any one of the dimensions where the bits differ, moving one dimension at a time until it reaches the destination.
	- the maximum distance possible is n hops
- adv: low latency for local communication, highly scalable, no single point of failure
- disadv: the number of ports per processor grows with n, which can be physically limiting
- figure below shows the hypercube structure for n=1,2,3:
  ![[hypercube_structure_123.png]]
- a representative of the hypercube architecture is the Intel iPSC computer complex
	- it consists of 128 (n=7) microcomputers, each node consists of a CPU, a floating-point processor, local memory and serial communication interface units.
## Comparision

| Features    | TSB                    | CS                              | MS                           | HC                                |
| ----------- | ---------------------- | ------------------------------- | ---------------------------- | --------------------------------- |
| Coupling    | Tightly                | Tightly                         | Tightly                      | Loosely                           |
| Performance | Low                    | very high                       | medium-high                  | high                              |
| Scalability | Poor                   | poor                            | good                         | excellent                         |
| Cost        | Low                    | very high                       | medium                       | medium-high                       |
| Complexity  | Low                    | high                            | medium                       | medium                            |
| Key Trait   | Single shared path     | dedicated, non-blocking paths   | multi-stage, may block       | direct links to neighbors         |
| Best For    | Small, low-cost system | demanding, medium-sized systems | larger shared-memory systems | large-scale distributed computing |
# 8.3 Interprocessor Communication and Synchronisation
- communication is how processors share data and updates
- synchronisation is how they coordinate their actions to avoid messing up shared data and updates
## Interprocess Communication
- mechanism that allows processors to exchange data and signals
### Shared Memory Communication (Tightly-Coupled Systems)
- concept: 
	- processors communicate by reading from and writing to a common, shared location.
	- most common method in tightly-coupled systems
- mechanism: the "Mailbox"
	1. A sending processor writes a message (data, request, status) into a designated memory block, called a 'mailbox'.
	2. the receiving processor periodically checks the mailbox for new messages
- disadvantage: this polling method can be slow and inefficient, as the receiver wastes time checking for messages that may not be there
- improvement: interrupt-driven method
	- the sending processor, after placing the message in the mailbox, sends an interrupt signal directly to the receiver processor.
	- the receiver doesn't have to constantly check and, it can attend to the message immediately
	- communication is much faster
### Message Passing Communication (Loosely-Coupled Systems)
- concept: in systems with no shared memory, processors communicate by sending discrete messages (packets) over an interconnection network, similar to how computers talk on LAN
- mechanism: one processor explicitly calls a procedure or sends a message to another processor's memory via I/O channels
- performance factors: the efficiency depends on
	- network topology
	- data link speed
	- the communication routing protocol

## Interprocessor Synchronisation
- synchronisation is a special form of communication where the data being exchanged is control information.
- used to ensure the correct order of operations and to protect shared resources
#### Need
- mutually exclusive access: 
	- prevents two or more processors from accessing and modifying a shared, writable resource (like a memory variable or a file) at the same time.
	- Without it, data corruption is inevitable
- critical section: 
	- a segment of code where a process accesses the shared resource. 
	- the fundamental rule of synchronisation is that only one process can be in its critical section at any time.
#### Mechanism: Mutual Exclusion with Semaphore
- Concept:
	- semaphore is a protected variable (or an abstract data type) used to control access to a shared resource. 
	- A binary semaphore (or mutex) is the most basic, with a value of 0 or 1.
- challenge: 
	- the act of checking the semaphore's value and then locking it must be indivisible, i.e. an atomic operation
	- If not, two processors could both read the free signal and both proceed to lock it simultaneously.
#### Hardware Solution: The Test-and-Set Instruction
- concept:
	- the CPU provides a special hardware instruction that performs the "check-and-lock" operation in a single, uninterruptible memory cycle
- working:
	- the TSL SEM (Test and Set Lock) instruction does two things automatically:
		1. reads the current value of the semaphore SEM into a register
		2. sets the value of SEM to 1 (locked), regardless of what it read
- processor logic:
- ```pseudocode
  Acquire_Lock:             
	  TSL REGISTER, SEM    # Atomically test and set the lock
	  CMP REGISTER, #0     # Was the lock FREE (0) when it checked?
	  JNE Acquire_Lock     # If not, it was already locked, so loop and try again
	  RET                  # If yes, the lock is acquired; enter critical section
	  
	Release_Lock:
		MOVE SEM, #0         # Set semaphore back to 0 (free)
		RET                  # exit critical section
  ```
- the "Hardware Lock": 
	- During the execution of the TSL instruction, the processor's "lock" signal is activated
	- it prevents any other processor or device from accessing memory bus until the instruction is complete
	- this enforces the atomicity
## Operating System Configurations for Multiprocessors
the OS must manage shared resources and processors. 3 primary models:

| Configuration             | Description                                                                                                                                                                                               | Pros & Cons                                                                                                                                                                                                       | Best For                                                                   |
| ------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------- |
| Master-Slave              | One processor, the master, is designated to run all OS kernel functions. Other processors (slaves) execute only user programs and make system calls to master                                             | **pros**: simple to implement, avoids OS data structure contention.<br>**cons**: master can become a bottleneck. if the master fails, the entire system fails                                                     | asymmetric systems where processors are not identical                      |
| Separate OS               | each processor has its own complete copy of the OS                                                                                                                                                        | **pros**: simple, no single point of failure for OS<br>**cons**: difficult to manage shared resources globally; no true-single system image. inefficient as each OS maintains its own tables                      | loosely-coupled systems (e.g., multicomputers)                             |
| Distributed (floating) OS | the OS routines are distributed among all processors. However, any specific OS function (e.g., memory manager) is performed only by one processor at a time, and this duty can "float" between processors | **Pros:** highly flexible and load-balanced; avoids a single master bottleneck; provides a true single-system image<br>**cons**: most complex to implement requires careful synchronisation of US data structures | Tightly-coupled multiprocessors (this is the model used in modern systems) |
