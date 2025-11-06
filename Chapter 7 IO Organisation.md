# 7.1 Peripheral devices
- set of input-output subsystem referred to as I/O
- provides efficient mode of communication between central system (processor and memory modules) and outside environment
- input or output devices attached to computer are called peripherals
- common peripherals:
	- keyboard, display units, printers
- some provide auxiliary storage: magnetic disks and tapes
- they are electro-mechanical and electro-magnetic devices
- can classify in 3 categories:
	- human readable: to communicate with humans: monitors, printers
	- machine readable: to communicate with equipment: magnetic disk, sensor
	- communication: to communicate with remote devices: modem, NIC
# 7.2 I/O Modules
- they:
	- interface to the system bus, or central switch (CPU and memory)
	- interface and control one or more peripheral devices
- i/o operations are accomplished through a wide assortment of external devices that provide a means of exchanging data between external environment and computer by a link to an I/O module.
- the link is used to exchange control status and data between i/o module and external devices
- figure:
  ![[Images/chapter-7/io_module.png]]
- functions:
	1. control & timing:
		- includes control and timing to coordinate the flow of traffic between internal resources and external devices
	2. processor communication:
		- command decoding: i/o module accepts commands from processor
		- data exchange: between processor and i/o module over bus
		- status reporting: processors need status of peripherals, provided by module
		- address recognition: i/o module must recognise one unique address for each peripheral it controls
	3. device communication:
		- data buffering: module must be able to operate at both device and memory speeds. if i/o devices rate are slower than memory, it buffers data so as to not tie up the memory in slower transfer operation
		- error detection: module is responsible for error detection such as mechanical and electrical malfunction reported by device
- structure:
		![[structure.png]]
	- i/o bus from the processor is attached to all peripheral interfaces
	- to communicate with the particular devices, the processor places a device address on the address bus
	- each interface contains an address decoder that monitors the address line.
		- when interface detects the particular device address, it activates the path between the data line and device that it controls
	- at the same time, that the address is made available in the address line, the processor provides a function code in the control line, including:
		- control command, output data and input data
- i/o module decisions:
	- hide or reveal device properties to CPU
	- support multiple or single device
	- control device functions or leave for CPU
	- also o/s decisions:
		- unix treats everything it can as a file
# 7.3 I/O Interface
**Need**:
- I/O interface provides a method for transferring information between internal storage (such as memory and CPU registers) and external I/O devices.
- Peripherals connected to a computer need special communication links for interfacing them with the central processing unit.
- The communication link resolves the following differences between the computer and peripheral devices
	- devices and signals
		- peripherals - electro-mechanical devices
		- CPU or Memory - electronic device
	- data transfer rate
		- peripherals - usually slower
		- C/M - faster than peripherals
		- some kinds of synchronisation mechanism maybe needed
	- unit of information
		- P-       Byte
		- C/M - Word
	- operating modes:
		- peripherals - autonomous, asynchronous
		- C/M - synchronous
- to resolve these differences, computer systems include special hardware components (i.e. interface) between the CPU and peripherals to supervise and synchronise all input and output interfaces.
**I/O Bus and Interface Modules**
- the I/O bus consists of data lines, address lines and control lines
- figure:
  ![[interface_module.png]]
- interface performs the following:
	- decodes the device address (device code)
	- decodes the commands (operation)
	- provides signals for the peripheral controller
	- synchronises the data flow and supervises the transfer rate between peripheral and C/M
- I/O commands that the interface may receive:
	- Control command: issued to activate the peripheral and to inform it what to do
	- status command: used to test various status conditions in the interface and the peripheral
	- output data: causes the interface to respond by transferring data from the bus into one of its registers
	- input data: to command interface to access data placed on the data bus, opposite of output data
- I/O versus memory bus
	- computer buses can be used to communicate with memory and I/O in three ways
		- use two separate buses, one for memory and other for I/O.
			- this way, all data, address and control lines will be separated for memory and i/o
		- use 1 common bus for both memory and i/o but have separate control lines
			- there is a separate read and write lines;
				- i/o read and i/o write for i/o and
				- memory read and memory write for memory
		- use a common bus for memory and i/o with common control line.
			- this configuration is called memory mapped.
- example:
  ![[example.png]]
	- information in each port can be assigned a meaning depending on the mode of operation of the I/O device
		- port A = Data, port B = Command, port C = Status
	- CPU initialises (loads) each port by transferring a byte to the control register
		- allows CPU to define the mode of operation of each port
		- programmable port: by changing the bits in the control register, it is possible to change the interface characteristics
- Isolated I/O versus Memory Mapped I/O
	- Isolated I/O
		- separate I/O read/write control lines in addition to memory read/write control lines
		- separate (isolated) memory and address space
		- distinct input and output instructions
	- Memory-mapped I/O
		- A single set of read/write control lines (no distinction between memory and i/o transfer)
		- memory and i/o address share the common address space which reduces memory address range available
		- no specific input or output instruction so the same memory reference instructions can be used for I/O transfers
		- Considerable flexibility in handling I/O operations
# 7.4 Modes of Transfer
- data transfer between the central computer and i/o devices may be handled in a variety of modes
- some modes use CPU as intermediate path, others transfer the data directly to and from the memory unit
- data transfer to and from peripherals may be handled in 1 of 3 ways:
## 7.4.1 Programmed I/O
- Flowchart:
  ![[programmed_io.png]]
- result of I/O instructions written in the computer program
- each data transfer is initiated by the instructions written in the CPU, and hence CPU is in continuous monitoring of the interface
- instructions:
	- input instruction is used to transfer data from I/O device to CPU, 
	- store instruction is used to transfer data from CPU to memory,
	- output instruction is used to transfer data from CPU to I/O device
- generally used in very slow speed computer
- is not efficient if the speed of CPU and I/O is different
- figure:
  ![[programmed_io_block.png]]
- operation of interface:
	- IO device places the data on the IO bus and enables its data valid signal
	- the interface accepts the data in the data register and sets the F bit of status register
	- enables the data accepted signal
	- data valid line is disabled by I/O device
	- CPU is in a continuous monitoring of the interface in which it checks the F bit of the status register.
		- if it is set, the CPU reads the data from data register and sets F bit to 0
		- if reset, the CPU remains monitoring the interface
	- interface disables the data  accepted signal
	- the system goes to initial state where next item of data is placed on the data bus
- flowchart for cpu program:
  ![[programmed_io_flowchart.png]]
- characteristics:
	- continuous CPU involvement
	- CPU slowed down to I/O sped
	- simple
	- least hardware
- can be described as polling or polled I/O or software driven I/O
## 7.4.2 Interrupt Driven I/O
- **Concept** and **Problem** with programmed I/O:
	- the problem with programmed I/O is that the processor has to wait a long time for I/O module of concern to be ready for either reception or transmission of data
	- the processor while waiting must repeatedly interrogate the status of I/O module.
	- As a result, the level of the performance of the system is severely degraded
	- an alternative is for the processor to issue an I/O command to a module and then go on to do some other useful work.
	- the i/o module will then interrupt the processor to request service when it is ready to exchange data
	- processor then executes the data transfer and then resumes its former processing.
	- the interrupt can be initiated either by software or by hardware.
- **Working Operation**:
	- I/O interface, instead of the CPU, monitors the I/O device
	- When the interface determines that the I/O device is ready for data transfer, it generates an *Interrupt Request* to the CPU
	- upon detecting an interrupt, CPU stops momentarily the task it is doing, branches to the service routine to process the data transfer, and then returns to the task it was performing
- **Summary**
	- CPU issued read command
	- I/O module gets data from peripheral whilst CPU does other work
	- I/O module interrupts CPU
	- CPU requests data
	- I/O module transfers data
- figure:
  ![[interrupt_io.png]]
- **Priority interrupts**:
	- determines which interrupt is to be served first when two or more requests are made simultaneously
	- also determines which interrupt are permitted to interrupt the computer while another is being serviced
	- higher priority interrupts can make requests while servicing a lower priority interrupt
- Priority Interrupt by **Software (Polling)**:
	- priority is established by the order of the polling devices (interrupt sources)
		- i.e. identify the highest-priority source by software means
	- one common branch address is used for all interrupts
	- program polls the interrupt sources in sequence
	- the highest-priority is tested first
	- flexible since it is established by software
	- low cost since it needs very little hardware
	- very slow
- Priority Interrupt by **Hardware**:
	- Require a priority interrupt manager which accepts all the interrupt requests to determine the highest priority request
	- fast since identification of the highest priority interrupt request is identified by the hardware.
	- fast since each interrupt source has its own interrupt vector to access directly to its own service routine
	- of two organisations of arrangement: **Daisy Chain (Serial) Priority** and **Parallel Priority**
	 1. **Daisy Chain Priority**
		 - figure:
		   ![[daisy_chain.png]]
		 - interrupt request from any device
		 - CPU responds by INTACK
		 - any device receives (INTACK) at PI (Priority In)  puts the VAD (Vector Address) on the bus
		 - among interrupt requesting devices 
			 - the only device which is physically closest to CPU gets INTACK 
			 - and it blocks INTACK to propagate to the next device
		 - one stage figure:
		   ![[daisy_chain_stage.png]]
	 2. **Parallel Priority**
		 - ![[parallel_priority.png]]
		 - IEN: Sets or Clear by instructions ION or IOF
		 - IST: Represents an unmasked interrupt has occurred.
		 - INTACT: enables tristate bus buffer to load VAD generated by the Priority Logic
		 - Interrupt register:
			 - each bit is associated with an Interrupt Request from different interrupt source (different priority level)
			 - each bit can be cleared by a program instruction
		 - mask register:
			 - mask register is associated with Interrupt Register
			 - Each bit can be set or cleared by an Instruction
## 7.4.3 Direct Memory Access
### Overview:
- Direct Memory Access (DMA) is a method that allows peripheral devices to transfer large blocks of data directly to and from memory without continuous CPU intervention.
- or: The transfer of data between the peripheral and memory without the interaction of CPU and letting the peripheral device manage the memory bus directly is termed as Direct Memory Access (DMA)
- particularly used for high-speed data transfers with devices like magnetic drums, disks, and tapes.
### How DMA Transfers data
1. CPU initialisation: CPU sets up the DMA controller by providing:
	- Starting memory address
	- Number of words to transfer
	- Transfer direction (read or write)
2. Direct Transfer: DMA controller manages data transfer directly between peripheral and memory
3. Completion: DMA controller interrupts CPU when transfer is complete
- Figure:
  ![[dma_signals.png]]
### DMA Controller
- Block diagram:
  ![[dma_controller_block.png]]
- The DMA controller communicates with the CPU through the data bus and control lines.
- Key Components:
	- Address Register: Holds current memory address (auto-increments after each transfer)
	- Word Count Register: Tracks remaining words to transfer (decrements after each transfer, if zero -> end of transfer)
	- Control Register: Specifies transfer mode (read/write)
	- Data Buffer: Temporarily holds data during transfer
	- DMA Select signal: Selects the controller
	- Register Select signal: Selects the register within the controlelr
- Operation Modes:
	1. DMA Burst: Transfers entire data block at once
	2. Cycle Stealing: 
		- Transfers one word at a time, returning bus control to CPU between transfers
		- CPU is usually much faster than I/O (DMA), thus CPU uses the most of the memory cycles
		- DMA Controller steals the memory cycles from CPU
		- for those stolen cycles, CPU remains idle
		- for those slow CPU, DMA Controller may steal most of the memory cycles which may cause CPU remain idle long time
### Bus Arbitration: DMA vs CPU
1. Control Signals:
	- Bus Request (BR): DMA requests control of system buses
	- Bus Grant (BG): CPU grants control to DMA controller
2. Arbitration Process:
	- DMA raises BR signal when ready to transfer
	- CPU completes current instruction, floats its buses (high-impedance state)
	- CPU asserts BG signal, granting bus control to DMA
	- DMA performs data transfer
	- DMA lowers BR when transfer complete
	- CPU resumes bus control by de-asserting BG
	- Figure: Transfer of data during arbitration:
	  ![[dma_transfer.png]]

	- Priority Handling: DMA has higher priority for memory access than CPU during transfer cycles
### I/O Technique Comparison

| Aspect          | Programmed                  | Interrupt-Driven                      | DMA                                |
| --------------- | --------------------------- | ------------------------------------- | ---------------------------------- |
| CPU Involvement | full control and monitoring | handles data transfer via ISR         | only initial setup                 |
| Data Transfer   | CPU moves each byte         | CPU moves each byte                   | direct device-memory transfer      |
| CPU Utilisation | very poor (polling)         | Better but still significant overhead | excellent (minimal involvement)    |
| Efficiency      | very low                    | medium                                | very high                          |
| Best for        | simple, low-speed devices   | medium-speed devices                  | high-speed, block transfer devices |
### DMA Configurations
1. Single Bus, Detached DMA
	- DMA controller separate from I/O controllers
	- All communication over system bus
	- adv: Simple design
	- disadv: bus contention (conflict) issues
2. Integrated DMA Controller
	- DMA functionality integrated into I/O controller
	- adv: Reduced bus traffic
	- disadv: more complex controller design
3. I/O Bus with DMA
	- separate I/O bus with DMA capability
	- adv: reduces load on system bus
	- disadv: higher system complexity
### Advantages over other methods:
1. **Drawbacks** of Alternative Methods:
	1. Programmed I/O: CPU constantly polls devices, wasting cycles
	2. Interrupt-driven I/O: Still requires CPU intervention for each data unit
2. How DMA **Overcomes** these:
	1. Minimal CPU involvement: CPU only involved in initialisation and completion
	2. Parallel Operation: CPU can execute other tasks during data transfer
	3. Higher Throughput: DMA eliminates intermediate CPU steps
	4. Efficient Bulk Transfers: Optimised for large data blocks
### Operation Sequence
1. Setup Phase: CPU programs DMA controller's registers
2. Transfer Phase:
	- DMA requests bus control via BR signal
	- CPU grants control via BG signal
	- DMA performs direct memory-to-peripheral transfers
3. Completion Phase
	- DMA interrupts CPU upon completion
	- CPU resumes normal operation
### Performance Consideration
- Cycle Stealing Impact: For slow CPUs, DMA may consume significant memory cycles causing CPU idle time
- Buffer management: Proper buffer allocation crucial for optimal performance
- System Architecture: Bus design affects DMA efficiency and potential bottlenecks
# 7.5 I/O Processors
- processor with direct memory access capability that communicates with I/O devices
- channel accesses memory by cycle stealing
- channel can execute a chanel program
- stored in the main memory
- consists of channel command word (CCW)
- each CCW specifies the parameters needed by the channel to control the I/O devices and perform data transfer operations
- CPU initiates the channel by executing a channel I/O class instruction and once initiated, channel operates independently of the CPU

- A computer may incorporate one or more external processors and assign them the task of communicating directly with the I/O devices
	- so that no each interface need to communicate with the CPU
- An I/O processor is a DMA capability that communicates with I/O  devices.
- IOP instructions are specifically designed to facilitate I/O transfer
- IOP can perform other processing tasks such as ALU, branching and code translation.
- Figure:
  ![[iop_block.png]]

## Why use an IOP?
IOP is a specialised processor that handles I/O operations, providing significant advantages:
- offloads I/O processing from main CPU
- enables true concurrent processing - CPU and IOP can work simultaneously
- Handles complex I/O trasks and device management
- Improves overall system performance by parallelising computation and I/O 
## CPU-IOP Communication Process
- memory unit acts as a message centre where each processor can exchange information
- operation of typical IOP is described with example by CPU and IOP communication:
  ![[cpu_iop_communication.png]]
- **Communication Sequence**:
	1. Path Testing
		- CPU sends instruction to test IOP path
		- IOP responds with status word in memory
	2. Status Checking
		- Status word bits indicate:
			- IOP overload condition
			- Device busy with another transfer
			- Device ready for I/O transfer
		- CPU checks status word to determine next action
	3. Transfer Initiation
		- If status is acceptable, CPU sends "start I/O transfer" instruction
		- CPU continues with other programs while IOP handles I/O
	4. Completion & Reporting
		- IOP sends interrupt request when transfer completes
		- CPU issues instruction to read IOP status
		- IOP places status report in specified memory location
		- Status word indicates: transfer completion or errors
- **Key Benefits**:
	- parallel processing: CPU executes programs while IOP handles I/O
	- reduced CPU overhead: CPU only involved in high level control
	- Efficient Resource Use: Specialised processor for I/O tasks
	- Scalability: Can manage multiple I/O devices efficiently
# 7.6 Data Communication Processor
## Purpose/Necessity
1. Purpose:
	- Manages multiple remote terminals connected via communication lines
	- Handles complex communication protocols that regular I/O processors can't manage
	- Specialised for long-distance data transmission with error control
	- Offloads communication processing from main CPU and standard I/O processors
	- Essential for distributed systems and network-connected environments
2. Essential due to:
	- Specialised communication handling
		- handling complex transmission protocols
		- handling various communication line types
	- error management:
		- implements multiple error detection methods
		- ensures data integrity over unreliable connections
	- scalability
		- supports multiple remote terminals
		- adapts to different transmission requirements
	- efficiency
		- offloads communication processing from main system
		- optimised for long-distance data transfer
## Core Functionality
- Distributes and collects data from multiple remote terminals
- Connects via telephone or other communication facilities
- Manages data links and communication protocols
## Communication Architectures

| Processor Communication        | Data Communication                     |
| ------------------------------ | -------------------------------------- |
| Uses Common bus system         | Uses dedicated wire pairs per terminal |
| shared data and control lines  | Point-to-point connections             |
| local peripheral communication | remote terminal communication          |
## Transmission methods
1. **Timing**
	1. Synchronous: Timed, coordinated transmission
	2. Asynchronous: Character-by-character transmission with start/stop bits
2. **Transmission Modes**:
	1. Simplex: One direction only, e.g., TV broadcasting
	2. Half Duplex: Both directions, but not simultaneously, e.g., walkie-talkie
	3. Full Duplex: Both direction simultaneously, e.g., Telephone
## Error Detection Mechanisms
- Parity Checking: Error detection in each character
- Checksum: Mathematical sum verification
- Longitudinal Redundancy Check (LRC): Vertical parity checking
- Cyclic Redundancy Check (CRC): Polynomial-based error detection
## Data Link and Protocols
- Data link: Communication lines, modems, and equipment for information transfer
- Protocols: Rules ensuring orderly information transfer in data links