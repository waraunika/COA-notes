## 1.1 Computer organisation and architecture
### 1.1.1 Computer Organisation
- Computer Organisation refers to the operational units and their interconnections that realise the architectural specifications

- Eg: instruction set, the number of bits used to represent various data types (eg: numbers, characters), I/O mechanisms, and techniques for addressing memory.
### 1.1.2 Computer Architecture

- Computer architecture refers to those attributes of a system visible to a programmer or put another way, those attributes that have a direct impact on the logical execution of a program

- Eg: hardware details transparent to the programmer such as control signals, interfaces between the computer and peripherals and memory technology used.
### 1.1.3 Differences:

| CA                                                                        | CO                                                                          |
| ------------------------------------------------------------------------- | --------------------------------------------------------------------------- |
| Definition                                                                | Definition                                                                  |
| refers to: ISA, number of bits for data, etc                              | refers to: control signals, interfaces, memory technology, etc              |
| The architecture could span years.                                        | The organization changes with modern technology                             |
| All Intel x86 family & IBM System/370 family share the same architecture. | Organization differs within the same architecture and between its versions. |
| eg: Is there a multiply instruction                                       | eg: Is there a hardware multiply unit or is it done by repeated addition.   |
### 1.1.4 Design Goals:
refers to the primary objectives that architects and engineers aim to achieve when developing a computer system. These typically include:

- Performance: The system should deliver high-speed execution of instructions and overall fast processing capabilities. This often involves explpoiting parallelism, increasing clock speeds, and optimizing instruction execution

- Const effectiveness: The architecture should achieve an optimal balance between performance and production cost. This includes considerations in chip manufacturing, system integration and energy consumption.

- Reliability and Fault Tolerance: Systems must be designed to handle faults gracefull and ensure continuous operation, especially in critical applications.

- Scalability & Upgradability: The architecture should allow for future enhancements or upgrades, enabling systems to changing technological requirements.

- Compatibility: Especially in system following a particular architecture (x86), backward compatibility is a key design goal to support legacy software and systems.
### 1.1.5 Performance Metrics:

to evaluate how well a computer system meets its design goals, especially performance, various metrics are employed:

- Instruction Execution Rate (MIPS/FLOPS): Measures how many millions of instructions per second or floating point operations per second the system can perform.

- Clock speed: The speed at which a processor executes instruction, typically measured in GHz. Higher clock speeds indicate faster systems.

- Throughput: The amount of work a system can perform in a given time period, relevant in multiuser or server environments.

- Latency: The time taken to complete a specific operation, such as a memory access or instruction cycle.

- Utilization and Efficiency: How effectively the system's resources (CPU, Memroy and I/O) are used during operation.

- Power consumption: How efficiently does the system perform based on how minimal power was provided.
## 1.2 Structure and Function

### 1.2.1 Structure
The structure of a computer system is a description of the static interconnections and relationships between its components. It defines "what is there" and "how it is built".

A structure must specify:
- The components: the physical or logical parts that make up the system, often being hierarchical.
- The interconnections (relationships): The pathways and protocols that allow components to communicate and interact. It comprises of list of parts and specification of their connectivity.
- The Hierarchical Nature: Structure is best understood as a series of layers, where the structure at one level is composed of the interconnected structures of the level below it.

In essence, structure is **blueprint** or the anatomy of the computer system.

### 1.2.2 Function
The function of a computer system is a description of the dynamic operation of each structural component. It defines "what it does" and "how it behaves over time".

A function must specify:
- The Operations (actions): The specific tasks that each component performs. This is about the transformation, movement or management of data and control signals.
- The sequence of events (The Behavior): The order in which operations occur. Function is inherently temporal.
- The casual relationship: How the operation of one component causes or enables or triggers the operation of another as dictated by the structure. eg: function of CU is to generate signals that cause the ALU to perform a specific arithmetic operation, which is a direct consequence of their structural interconnection.  

In essence, function is the **algorithm** or the physiology of the computer system. 

### 1.2.3 Four Functions
- Data processing: Computer must be able to process data which may take a wide variety of forms and the range of processing.
- Data storage: Computer stores data either temporarily or permanently
- Data movement: Computer must be able to move data between itself and the outside world.
- Control: There must be a control of the above three functions.

![Functional View of Computer|600](image.png)
![Data movement and Data Storage|600](image-1.png)
![Data Storage between processing and i/o|600](image-2.png)

### 1.2.4 Structural Components
Four main structural components of a computer between Peripherals and Communication lines:
- CPU
- Main Memory
- I/O
- System Interconnections

#### 1.2.4.1 CPU
- CU
- ALU
- Registers
- CPU interconnections

## 1.3 Designing For Performance
- Microprocessor Speed
	- Pipeling
	- Onboard cache, onboard L1 and L2 cache
	- Branch prediction: The processor looks ahead in the instruction code fetched memory and predicts which branches, or group of instructions are likely to be processed next.
	- Data flow analysis: The processor analyses which instructions are dependent on each other's results, or data, to create an optimised to prevent delay.
	- Speculative execution: Using branch prediction and data flow analysis, some processors speculatively execute instructions ahead of their actual appearance in the program execution, holding the results in temporary locations.
- Performance Mismatch
	- Processor speed increased
	- Memory capacity increased
	- Memory speed lags behind processor speed

Explanations:
- Microprocessor Speed:
	- The raw speed of mpu will not achieve its potential unless it is fed a constant stream of work to do in the form of computer instructions.
	- Anything that gets in the way of that smooth flow undermines the power of the processor.
	- While chip-makers have been busy learning how to fabricate chips of greater and greater density, the processor designer must come up with even more elaborate techniques to feed the mpu
	- Among the techniques built into contemporary processors are:
		- Branch prediction: mpu looks ahead in the software and predicts which branch or groups of instructions are likely to be processed next. If guessed correctly for most of the time, it can prefetch the correct instructions and buffer them, so that processor is kept busy.
		- Dataflow Analysis: mpu analyses which instructions are dependent on each other's results, or data, to create an optimised schedule of instructions
		- Speculative execution: using branch prediction and data flow analysis, some processors speculatively execute instructions ahead of the their actual appearance in program execution, holding results in temporary locations.
		  
- Performance Balance:
	- The equilibrium between different components of a computer system, especially the processor and memory, such that neither becomes a bottleneck for the other.
	- The requirement stems from a simple fact: processor speeds have increased much faster than memory speed over the years. It is imbalanced as processor's efficiency is thwarted by the limited speed at which the processor gets data from main memory.
	- Main two points:
		- Processor-memory gap: CPU performance has been growing, thanks to architecture/organisation structure like pipelining, superscalar architecture, etc. For memory, the progress isn't the same. DRAM latency and bandwidth improvement aren't keeping up. This difference creates processor-memory gap.
		- Efficient utilisation: Even with a high spec CPU, if the CPU has a long time to wait before a memory fetch is completed. Hence, the system performance is now unable to reflect CPU's potential as CPU does no real work.

There are ways to improve this balance and maintain it:
- Cache memory: This is the front line in the war against latency. Cache reduces the average time to access memory, by keeping frequently used data close to CPU.
- More levels of cache: Modern systems stack L1, L2, L3 as layers of cache memory for further efficiency.
- Wider memory bus: This way, more bits can be transferred within one clock cycle and lets the memory to send or receive bulkier data
- Prefetching: analyse ahead of time to fetch required data and load it into CPU for faster computation.
- Increasing DRAM bandwidth: Techniques like interleaving and using multiple memory banks help fetch more data per unit time.

## 1.4 Interconnection Structures
The collection of paths connecting the various modules is called the interconnecting structure.

- Computer consists of a set of components or modules of 3 basic types: CPU, memory, I/O, that communicate with each other. In effect, a computer is a network of basic modules.
- Thus, there must be paths for connecting the modules together.
- The collection of paths connecting the various modules is called the interconnection structure.

### 1.4.1 Memory Connection
- Typically a memory module will consist of N words of equal length. Each word is assigned a unique numerical address (0, 1, 2, ..., N-2, N-1). A word of data can be read from or written into the memory. The nature of the operation is indicated by R/W control signals. The location for operation specified by an address.
- The connection has to:
	- Receive and send data
	- Receive addresses of location for words in memory
	- Receive control signals for:
		- Read
		- Write
		- Timing

![Memory Module|600](memory_module.png)

### 1.4.2 I/O Connection
* From an internal P.O.V., I/O is functionally similar to memory. There are two operations, Read and Write. Furthermore, I/O module may control more than one external device. We can refer to each of the interfaces to an external device as a port and give each a unique address. External data path for the I/O of data with external/peripheral devices. And an I/O module may be able to send interrupt signals to the CPU.
* The connections are:
	* Output:
		* Receive data from computer
		* Send data to peripheral
	* Input
		* Receive data from peripheral
		* Send data to computer
	* Control signals:
		* Receive control signals from computer
		* Send control signals to peripheral, eg: spin disk
	* Receive address from computer, eg: port number to identify peripheral
	* Send Interrupt signals (control)

![I/O Module | 600][memory_module.png]

### 1.4.3 CPU Connection
- The CPU reads in instructions and data, writes out data after processing and uses control signals to control the overall operation of the system. It also receives interrupt signals.
- The connections are:
	- Reads instruction and data
	- Writes out data after processing
	- Sends control signals to other units
	- Receives and acts on interrupts

![CPU Module | 600](cpu_module.png)

## 1.5 Bus Interconnection

### 1.5.1 Interconnection networks:
* A bus interconnection network is a collection of processing elements (processors) and communication elements (buses)/
* The processors produce and/or take messages and the buses provide communication channels to exchange messages and the buses provide communication channels to exchange connecting two or more devices, with a characteristic that it is a shared medium.
* A bus that connects major computer components (CPU, memory, I/O) is called a system bus. The mos common computer interconnection structures are based on the use of one or more system buses.
* Usually broadcast signals i.e. all components connected by the bus see the signal
* Often grouped:
	* A number of channels in one bus
	* eg: 32 bit data bus is 32 separate single-bit channels
- Power lines may not be shown
- There are a number of possible interconnection systems
- Single and multiple bus structure are most common
- eg: Control/Address/Data bus (for PC)
- eg: Unibus (for DEC-PDP)
- Lots of devices on one bus leads to:
	- Propagation delays
	- Long data paths mean that co-ordination of bus use can adversely affect performance if aggregate data transfer approaches bus capacity.
- Most systems use multiple buses to overcome these problems

![Bus Interconnection Scheme | 600][bus_interconnection.png]

### 1.5.2 Width of address bus
- Bus can be classified into three functional groups: data, address and control lines.
- Say, for an 8-bit address bus, the addresses from 00000000 to 11111111 can be uniquely identified by varying each bit.
- or, an MSB may be separated as the bit to define modules. like, bit 7 being 0 defines $2^7$ addresses to present in module 0 and being 1 define the other $2^7$ addresses present in module 1.
- Clearly, the width off address bus represents the maximum possible memory capacity of the system.
### 1.5.3 Elements of Bus Design
1. Bus Types
	1) Dedicated:
		- Separate data & address lines
	2) Multiplexed:
		- Shared lines
		- Address valid or data valid control line
		- Advantage: fewer lines
		- Disadvantage: more complex control, ultimately performance is hindered.
2. Bus Arbitration
	- More than one module controlling the bus (eg: CPU and DMA controller)
	- Only one module may control bus at one time
	- Arbitration may be centralised or distributed
		1) Centralised Arbitration
			- Single hardware device controlling bus access (Bus Controller, Arbiter)
			- May be part of CPU or separate
		2) Distributed Arbitration
			- Each module may claim the bus
			- Control logic on all modules
3. Timing
	- Co-ordination of events on bus
	- It should be synchronous to have:
		- Events determined by clock signals
		- Control Bus includes clock line
		- A single 1-0 is a bus cycle
		- All devices can read clock line
		- Usually sync on leading edge
		- Usually a single cycle for an event
4. Bus Width
	- Address: Width of address bus has an impact on system capacity (wider bus means greater range of locations for transfers)
	- Data: Width of data bus has an impact on system performance (wider bus means more bits can be transferred at a time)
5. Data Transfer Type
	- Read
	- Write
	- Read-modify-write
	- Read-after-write
	- Block

### 1.5.4 Hierarchy
* A great number of devices on a bus will cause performance to suffer:
	* Propagation delay - the time it takes for devices to coordinate the use of the bus
	* The bus may become a bottleneck as the aggregate data transfer demand approaches the capacity of the bus (in available transfer cycles/second).
#### 1.5.4.1 Traditional Hierarchical Bus Architecture
* Use of a cache structure insulates CPU from frequency access to main memory.
* Main memory can be moved off local bus to a system bus.
* Explain bus interface
	* buffers data transfers between system bus and I/O controllers on expansion bus
	* insulates memory-to-processor traffic from I/O traffic
* Traditional Hierarchical Bus Architecture example:
	![Traditional Hierarchical Bus Architecture | 600][traditional_hierarchy.png]

#### 1.5.4.2 High-performance Hierarchical Bus Architecture
- Traditional hierarchical bus breaks down as higher and higher performance is seen in the I/O devices
- Incorporates a high-speed bus
	- Specifically designed to support high-capacity I/O devices
	- Brings high-demand devices into closer integration with the processor and at the same time is independent of the processor.
	- Changes in processor architecture do not affect the high-speed bus, and vice versa.
- Sometimes known as a mezzanine architecture.
- High-performance Hierarchical Bus Architecture example: ![high-performance Hierarchical Bus Architecture | 600][high-performance_hierarchy.png]

## 1.6 PCI (Peripheral Component Interconnect)
- PCI is a popular high bandwidth, processor independent bus that can function as mezzanine or peripheral bus.
- PCI delivers better system performance for high speed I/O subsystems (graphic display adaptors, network interface controllers, disk controllers, etc.)
- PCI is designed to support a variety of microprocessor based configurations including both single and multiple processor system.
- It makes use of synchronous timing and centralised arbitration scheme.
- PCI may be configured as 32 or 64-bit bus.
- Current standard:
	- upto 64 data lines at 33 MHz
	- requires few chips to implement
	- supports other buses attached to PCI bus
	- public domain, initially developed by Intel to support Pentium-based systems
	- supports a variety of microprocessor-based configurations, including multiple processors.
	- uses synchronous timing and centralised arbitration.

![PCI Typical Desktop System | 600][pci.png]
Note: *Bridge acts as a data buffer so that the speed of the PCI bus may differ from that of the processor's I/O capability*