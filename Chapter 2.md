## 2.1 CPU Structure and Function
The part of the computer that performs the bulk of data processing operations is called the CPU and is the central component of a digital computer.

### 2.1.1 Components of CPU

#### 2.1.1.1 Processor Unit
- Consists of ALU, a number of registers and internal buses between register and ALU.
- The registers communicate each other not only for direct data transfer but also while performing various micro-operations.
- One way to organise the processor unit could be:
		![Processor Unit | 600][processor.png]
- Here in the figure, two sets of multiplexers select register which perform input data for ALU.
- A decoder selects the destination register by enabling its load input.
- The function select (SELA) in ALU determines the particular operation to be performed.
- For an example to perform the operation: R3 <- R1 + R2
	1. MUX A selector (SELA): to place the content of R1 into bus A. (say 001 for R1)
	2. MUX B selector (SELB): to place the content of R2 into bus B. (say 010 for R2)
	3. ALU operation selector (OPR): to provide arithmetic addition A + B. (say 00000 for addition)
	4. Decoder destination selector (SELD): to transfer the content of the output bus into R3. (011 for R3)
#### 2.1.1.2 Control Unit
- consists of program counter, instruction register, timing and control logic.
- Two types: hardwired or micro-programmed.
	1. Hardwired: register decodes and a set of gates are connected to provide the logic that determines the action required to execute various instructions.
	2. Micro-programmed: uses a control memory to store micro instructions and a sequence to determine the order by which the instructions are read from control memory.
- The control unit decides what the instructions mean and directs the necessary data to be moved from memory to ALU.
- must communicate with both ALU and main memory and coordinates all activities of processor unit, peripheral devices and storage devices. It can be characterised on the basis of design and implementation by:
	- Defining basic elements of the processor
	- Describing the micro-operation that processor performs
	- Determining the function that the control unit must perform to cause the micro-operations to be performed.
- Control unit must have inputs that allow determining the state of system and outputs that allow controlling the behaviour of system.

- The input to control unit are:
	- Flag: flags are headed to determine the status of processor and outcome of previous ALU operation.
	- Clock: All micro-operations are performed within each clock pulse. This clock pulse is also called as processor cycle time or clock cycle time.
	- Instruction Register: The op-code of instruction determines which micro-operation to perform during execution cycle.
	- Instruction Register: The op-code of instruction determines which micro-operation to perform during execution cycle.
	- Control signal from control bus: The control bus portion of system bus provides interrupt, acknowledgement signals to control unit.

- The outputs from control unit are:
	- Control signal within processor: These signals causes data transfer between registers, activate ALU functions.
	- Control signal to control bus: These are signals to memory and I/O module. All these control signals are applied directly as binary inputs to individual logic gate.
- Figure:		![Control Unit | 600][control_unit.png]
### 2.1.2 Organisation of CPU
#### 2.1.2.1 Processor Organisation
![CPU with the System Bus | 340][cpu_with_system_bus.png] ![Internal Structure of the CPU | 350][internal_structure_of_cpu.png]
				CPU with system bus                                          Internal Structure of CPU
Components of CPU: 
- ALU: does the actual computation or processing of data
- CU: controls the movement of data and instructions into and out of the CPU and controls the operation of the ALU.
#### 2.1.2.2 Register Organisation
- Registers are at top of the memory hierarchy. They are of two types
	1. User-Visible registers - enable the machine or assembly language programmer to minimise main-memory references by optimising the use of registers
	2. Control and Status registers - used by the control unit to control the operation of the CPU and by privileged. OS programs to control the execution of programs.
1. User-Visible Registers:
	- Categories of use cases:
		1. General purpose registers - for variety of functions
		2. Data registers - hold data
		3. Address registers - hold address information
		4. Segment pointers - hold base address of the segment in use
		5. Index registers - used for indexed addressing and may be auto indexed
		6. Stack Pointer- points to top of a stack for push, pop and other stack instructions
		7. Condition Codes (flags)
	* Design Issues:
		1. Completely general-purpose registers or specialised use?
			- Specialised registers save bits in instructions because their use can be implicit
			- General-purpose registers are more flexible
			- Trend is toward use of specialised registers
		2. Number of registers provided?
			- More registers require more operand specifier bits in instructions
			- 8 to 32 registers appear optimum (RISC systems use hundreds, but are a completely different approach)
		3. Register Length?
			- Address registers must be long enough to hold the largest address
			- Data registers should be able to hold values of most data types
			- Some machines allow two contiguous registers for double-length values
		4. Automatic or manual save of condition codes
		- Condition restore is usually automatic upon call return
		- Saving condition code registers may be automatic upon call instruction, or may be manual.
2. Control and Status Registers
	- Essential to instruction execution
		1. Program Counter (PC)
		2. Instruction Register (IR)
		3. Memory Address Register (MAR) - usually connected directly to address line of bus
		4. Memory Buffer Register (MBR) - usually connected directly to data lines of bus
	- Program Status Word (PSW) - also essential. Common fields or flags contained are:
		1. Sign - sign bit of last arithmetic operation
		2. Zero - set when result of last arithmetic operation is 0
		3. Carry - set if last op resulted in a carry into or borrow out of a high-order bit
		4. Equal - set if a logical compare result is equality
		5. Overflow - set when last arithmetic operation caused overflow
		6. Interrupt Enable/Disable - used to enable or disable interrupts
		7. Supervisor - indicates if privileged operations can be used
	- Other optional registers
		1. Pointer to a block of memory containing additional status info (like process control blocks)
		2. An interrupt vector
		3. A system stack pointer
		4. A page table pointer
		5. I/O registers
	- Design issues
		1. Operating system support in CPU
		2. How to divide allocation of control information between CPU registers and first part of main memory (usual trade-offs apply)
![Microprocessor Register Organisation | 600][register_organisation.png]

### 2.1.3 Instruction Cycle
Basic instruction cycle contains the following sub-cycles
- fetch -read next instruction from memory into CPU
- execute - interpret the opcode and perform the indicated operation
- interrupt - if interrupts are enabled and one has occurred, save the current process state and service the interrupt
![Instruction Cycles | 600][instruction_cycles.png]
![Instruction Cycle State Diagram | 600][instruction_cycle_state_diagram.png]
#### The Indirect Cycle
- can be thought as another sub-cycle
- may require just another fetch (based upon last fetch)
- might also require arithmetic, like indexing
![Instruction Cycle with indirect][indirect_cycle.png]
#### Data Flow
- Exact sequence depends on CPU design
- We can indicate sequence in general terms, assuming CPU employs:
	1. a memory address register (MAR)
	2. a memory buffer register (MBR)
	3. a program counter (PC)
	4. an instruction register (IR)
#### Fetch Cycle Data Flow
- PC contains address of next instruction to be fetched
- This address is moved to MAR and placed on address bus
- Control unit requests a memory read
- Result is
	1. placed on data bus
	2. result copied to MBR
	3. then moved to IR
- Meanwhile, PC is incremented
![Data Flow, Fetch Cycle][data_flow_cycle.png]
												Fig: Data Flow, Fetch Cycle
example:
- t1: MAR <- (PC)
- t2: MBR <- Memory
- t3: IR (Address) <- (MBR(Address))
#### Indirect cycle data flow
- Decodes the instruction
- After fetch, control unit examines IR to see if indirect addressing is being used. If so:
	- Rightmost n bits of MBR (the memory reference) are transferred to MAR
- Control unit requests a memory read, to get the desired operand address into the MBR.
![Data Flow, Indirect Cycle | 600][data_flow_indirect.png]
* Example:
	* t1: MAR <- (IR (Address))
	* t2: MBR <- Memory
	* t3: IR (Address) <- (MBR (Address))
#### Execute Cycle Data Flow
- not simple and predictable, like other cycles
- Takes many forms, since form depends on which of the various machine instruction is in the IR
- May involve
	- transferring data among registers
	- read or write from memroy or I/O
	- invocation of the ALU
- For example: ADD R$_{1}$, X 
	- t1: MAR <- (IR(Address))
	- t2: MBR <- Memory
	- t3: R$_1$ <- (R$_1$) + (MBR)
#### Interrupt Cycle Data flow
- Current contents of PC must be saved (for resume after interrupt), so PC is transferred to MBR to be written to memory
- Save location's address (such as a stack ptr) is loaded into MAR from the CU.
- PC is loaded with address of interrupt routine (so next instruction cycle will begin by fetching appropriate instruction)
- example: 
	- t1: MBR <- (PC)
	- t2: MAR <- save_address
	-       PC <- Routine_address
	- t3: Memory <- (MBR)
## 2.2 ALU
### 2.2.1 Design
There are three stages in design of ALU:
1. Design the arithmetic section
	- The basic component of arithmetic ckt is a parallel adder which is constructed with a number of full adder circuits connected in cascade.
	- By controlling the data inputs to the parallel adder, it is possible to obtain different types of arithmetic operations.
	- Below figure shows the arithmetic circuit and its functional table.
	![Arithmetic Example | 650][arithmetic_design.png]
2. Design the logical section
	- The basic components of logical circuit are AND, OR, XOR, and NOT gate circuits connected accordingly. Below figure shows a circuit that generates four basic logic micro-operations.
	- ![Bock Diagram of Logic Unit | 450][block_logic_unit.png] ![Functional Table | 450][function_logic_unit.png]
	- It consists of four gates and a multiplexer.
	- Each of four logic operations is generated through a gate that performs the required logic.
	- The two selection input S1 and S0 choose one of the data inputs of the multiplexer and directs its value to the output.
	- Functional table lists the logic operations.
3. Combine these 2 section
	- Below figure shows a combined circuit of ALU where n data input from A are combined with n data input from B to generate the result of an operation at the G output line.
	  ![Block Diagram of ALU | 600][block_alu.png]
	- ALU has a number of selection lines used to determine the operation to be performed.
	- The selection lines are decoded with the ALU so that selection lines can specify distinct operations.
	- The mode select S$_2$ differentiate between arithmetic and logical operations.
	- The two functions select S$_1$ and S$_0$ specify the particular arithmetic and logic operations to be performed.
	- With three selection lines, it is possible to specify arithmetic operation with S$_2$ at 0 and logical operation with S$_2$ at 1.
### 2.2.2 Example of 2-bit ALU (addition, AND, OR, XOR)
![ALU Example | 600][alu_example.png]
## 2.3 Instruction Formats
- Collection of ordered steps form a computer 'program'. These ordered steps are called instructions.
- Instructions are stored in central memory locations and are executed sequentially one at a time.
- The CU reads an instruction from a specific address in memory and executes it.
- It then continues by reading the next instruction in sequence and executes it until the completion of the program.

- Computer usually has a variety of Instruction Code Formats.
- An n bit instruction:
	- that k bits in the address field, and
	- m bits in operation code field can
	- can address 2$^k$ location directly and specify 2$^m$ different operation.

- The bits of the instruction are divided into groups called fields
- The most common fields in instruction formats are:
	- An **Operation Code** field that specifies the operation to be performed.
	- An **Address** field that designates a memory address or a process register
	- A **Mode** field that specifies the way the operand or the effective address is determined.
![Instruction Format with mode field | 600][IF_with_mode.png]
- Opcode of an instruction is a group of bits that define various processor operations such as add, subtract, complement, shift etc.
- The bits that define the mode field of an instruction code specify a variety of alternatives for choosing the operands from the given address,
- Operation specified by an instruction is executed on some data stored in the processor register or in the memory location.
- Operands residing in memory are specified by their memory address. Operands residing in processor register are specified with a register address.
#### Types of instruction
- Computers may have instructions of several different lengths containing varying number of addresses.
- The number of address fields in the instruction format of a computer depends on the internal organisation of its registers.
- Most computers fall into one of three types of CPU organisations:
	1. Single accumulator organisation: All the operations are performed with an accumulator register. The instruction format in this type of computer uses on address field. For example: ADD X, where X is the address of the operand.
	2. General register organisation: The instruction format in this type of computer needs three register address fields. For example: ADD R1, R2, R3
	3. Stack organisation: The instruction in a stack computer consists of an operation code with no address field. This operation has the effect of popping the top 2 numbers from the stack, operating the numbers and pushing the sum into the stack. For example: ADD
##### Three address Instruction
- With this type of instruction, each instruction specifies two operand location and a result location.
- A temporary location T is used to store some intermediate results so as not to alter any operand location.
- The three address instruction format requires a very complex design to hold the three address references.
- Adv: It results in short programs when evaluating arithmetic expressions
- The instructions require too many bits to specify 3 addresses.
- Format: Op X, Y, Z ;    X <- Y Op Z
- Example: X = (A+B)* (C+D)
	- ADD R1, A, B;        R1 <- M[A] + M[B]
	- ADD R2, C, D;       R2 <- M[C] + M[D]
	- MUL X, R1, R2;      M[X] <- R1 * R2
##### Two address instruction
- Two-address instructions are the most common in commercial computers.
- Here each address field can specify either a processor register, or a memory word.
- One address must do double duty as both operand and result.
- The two address instruction format reduces the space requirement.
- To avoid altering the value of an operand, a MOV instruction is used to move one of the values to a result or temporary location T, before performing the operation.
- Format: Op X, Y      ;      X <- X Op Y
- Example: X = (A+B)* (C+D)
	- MOV R1, A;       R1 <- M[A]
	- ADD R1, B;        R1 <- R1 + M[B]
	- MOV R2, C;      R1 <- M[C]
	- MUL R1, R2;     R1 <- R1 * R2
	- MOV X, R1;       M[X] <- R1
##### One Address Instruction
- It was generally used in earlier machine with the implied address being a CPU register known as accumulator.
- The accumulator contains one of the operand and is used to store the result.
- One-address instruction uses an implied accumulator (AC) register for all data manipulation. 
- All operations are done between the AC register and a memory operand.
- We use LOAD and STORE instruction for transfer to and from memory and AC register.
- Format: Op X     ;    Ac <- Ac Op X
- Example: X = (A+B) * (C+D)
	- LOAD A;        Ac <- M[A]
	- ADD B;           Ac <- Ac + M[B]
	- STORE T;       M[T] <- Ac
	- LOAD C;         Ac <- M[C]
	- ADD D;           Ac <- Ac + M[D]
	- MUL T;           Ac <- Ac * M[T]
	- STORE X;       M[X] <- Ac
##### Zero Address Instruction
- It does not use address field for the instruction like ADD, SUB, MUL, DIV, etc.
- The PUSH and POP instructions, however, need an address field to specify the operand that communicates with the stack.
- The name "Zero" address is given because of the absence of an address field in the computational instruction.
- Format: Op    ;        TOPS <-    TOS   Op (TOS - 1)
- Example: (A+B) * (C+D)
	- PUSH A;      TOS <- A
	- PUSH B;      TOS <- B
	- ADD;            TOS <- (A+B)
	- PUSH C;      TOS <- C
	- PUSH D;      TOS <- D
	- ADD;            TOS <- C+D
	- MUL;            TOS <- (C+D) * (A+B)
	- POP X;         M[X] <- TOS 
## Data Transfer and Manipulation
#### Data Transfer
- Data transfer instructions cause transfer of data from one location to another without changing the binary information.
- The most common transfer are between the:
	- Memory and Processor registers
	- Processor registers and input output devices
	- Processor registers themselves
- Typical data transfer instructions are:
	- Load          LD
	- Store         ST
	- Move         MOV
	- Exchange  XCH
	- Input          IN
	- Output       OUT     
	- Push          PUSH       
	- Pop            POP    
#### Data Manipulation Instruction
- perform operations on data and provide the computation capabilities for the computer.
- perform arithmetic, logic and shift operations.
##### Arithmetic Instruction
* Increment                             INC
* Decrement                           DEC     
* Add                                       ADD       
* Subtract                                SUB       
* Multiply                                 MUL    
* Divide                                    DIV  
* Add with carry                      ADDC     
* Subtract with Burrow           SUBB    
* Negate (2's complement)    NEG
##### Logical and Bit Manipulation Instructions
* Clear                          CLR      
* Complement             COM                   
* AND                           AND     
* OR                              OR  
* Exclusive-OR             XOR                   
* Clear Carry                CLRC                
* Set carry                    SETC            
* Complement carry    COMC        
* Enable interrupt         EI      
* Disable interrupt        DI
##### Shift Instructions
* Logical shift right                  SHR 
* Logical shift left                    SHL 
* Arithmetic shift right             SHRA  
* Arithmetic shift left               SHLA  
* Rotate right                           ROR  
* Rotate left                             ROL   
* Rotate right through carry   RORC
* Rotate left through carry     ROLC
##### Program Control Instructions
- Branch         BR        
- Jump           JMP      
- Skip             SKP    
- Call              CALL   
- Return         RET        
- Compare     CMP
- Test             TST
## Addressing Modes
- specifies a rule for interpreting or modifying the address field of the instruction before the operand is actually referenced.
- Computers use addressing mode techniques for the purpose of accommodating the following purposes:
	- To give programming versatility to the user by providing such facilities as pointers to memory, counters for loop control, indexing of data and various other purposes.
	- To reduce the number of bits in the addressing field of the instructions
		- Other computers use a single binary for operation & Address mode
		- The mode field is used to locate the operand
		- Addressing field may designate a memory address or a processor register
		- There are 2 modes that need no address field at all (Implied and Immediate modes)
* Effective Address (EA):
	- The effective address is defined to be the memory address obtained from the computation dictated by the given addressing mode.
	- The effective address is the address of the operand in a computational-type instruction.
### Implied Addressing Mode
* The operands are specified implicitly in the definition of the instruction.
* For example: CMA - "Complement accumulator" is an implied mode instruction because the operand in the accumulator register is implied in the definition of the instruction.
* In fact, all register reference instructions that use an accumulator are implied-mode instructions.
* Adv: no memory reference
* Disadv: limited operand
* ![Implied Addressing Mode uses only OpCode | 300][implied.png]
### Immediate Addressing mode:
- The operand is specified in the instruction itself.
- has an operand field rather than an address field.
- operand field contains the actual operand to be used in conjunction with the operation specified in the instruction.
- These instructions are useful for initialising register to a constant value;
- Eg: MVI B, 50H
- *When the address field specifies a processor register, the instruction is said to be in register mode.*
- Adv: No memory reference
- Disadv: limited operand
- ![Immidate Addressing Mode requires OpCode and operand | 300][immediate.png]
### Register direct addressing
- The operands are in registers that reside within the CPU.
- The particular register is selected from the register field in the instruction.
- Eg: MOV A, B
- Adv: no memory reference
- Disadv: limited address space
- ![Register Direct uses Register address as operand | 350][register_direct.png]
### Register indirect addressing
- The instruction specifies a register in CPU whose contents give address of the operand in the memory.
- The selected register contains the address of the operand rather than the operand itself.
- Before using a register indirect mode instruction, the programmer must ensure that the memory address of the operand is placed in the processor register with a previous instruction.
- Eg: LDAX B
	- Adv: Large address space:
		- The address field of the instruction uses fewer bits to select a register than would have been required to specify a memory address directly.
	- Disadv: Extra memory reference
- ![Register Indirect uses contents of register as address | 360][register_indirect.png]
### Auto increment or auto decrement addressing
- This is similar to register indirect mode except that the register is incremented or decremented after (or before) its value is used to access memory.
- When the address stored in the registers refers to a table of data in memory, it is necessary to increment or decrement the registers after every access to the table.
- This can be achieved by using the increment/decrement instruction. In some computers, it is automatically accessed.
- The address field of an instruction is used by the control unit in the CPU to obtain the operands from memory.
- Sometimes, the value given in the address field is the address of the operand, but sometimes it is the address from which the address has to be calculated.
### Direct Addressing mode
- The EA is equal to the address part of the instruction.
- The operand resides in memory and its address is given directly by the address field of the instruction.
- Eg: LDA 4000H
	- Adv: Simple
	- Disadv: limited address field
- ![Direct addressing uses Address to find operand | 370][direct_addressing.png]
### Indirect Addressing Mode
- The address field of the instruction gives the address where the effective address is stored in memory.
- Control unit fetches the instruction from the memory and uses its address part to access memory again to read the effective address.
	- Adv: Flexibility
	- Disadv: Complexity
- ![Indirect addressing mode houses address which points to the address of operand | 380][indirect_addressing.png]
### Displacement Addressing
- powerful mode of addressing
- combines capabilities of direct addressing and register indirect addressing
- address field of instruction is added to the content of specific register in the CPU.
	- Adv: flexible
	- Disadv: complexity
- ![Displacement uses register's value to add to address to get operand | 390][displacement_addressing.png]
### Relative Addressing mode
- content of program counter is added to the address part of the instruction in order to obtain the effective address.
- the address part of the instruction is usually a signed number (+ve/-ve number)
- When the number is added to the content of the program counter, the result produces an effective address whose position in memory is relative to the address of the next instruction.
- EA = PC + A
### Indexed Addressing Mode
- content of an index register (XR) is added to the address part of the instruction to obtain the EA.
- The index register is a special CPU register that contains an index value.
- Note: *If an index-type instruction does not include an address field in its format, the instruction is automatically converted to the register indirect mode of operation.*
- EA = XR + A
### Base Register Addressing mode
- the content of a base register (BR) is added to the address part of the instruction to obtain the effective address.
- This is similar to the indexed addressing mode except that the register is now called a base register instead f the indexed register.
- The base register addressing mode is used in computers to facilitate the relocation of programs in memory i.e.
	- when programs and data are moved from one segment to another.
- EA = BR + A
### Stack Addressing Mode
- The stack is the linear array of locations
- sometimes, referred to as push down list or LIFO queue.
- stack pointer (TOS) is maintained in register.
- address is implicitly used by means of TOS
- ![Stack uses TOS as EA][stack_addressing.png]
### Examples
![Numerical Example for Addressing Modes][addressing_examples.png] 
## RISC and CISC

| S.N. | RISC                                                             | CISC                                                                                              |
| ---- | ---------------------------------------------------------------- | ------------------------------------------------------------------------------------------------- |
| 1    | Simple instructions taking one cycle                             | Complex instructions taking multiple cycles                                                       |
| 2    | Only load and store memory references                            | Any instructions may reference memory                                                             |
| 3    | Heavily pipelined                                                | Not/less pipelined                                                                                |
| 4    | Multiple register sets                                           | Single register set                                                                               |
| 5    | Complexity is in compiler                                        | Complexity is in micro-programming                                                                |
| 6    | Instructions executed by hardware                                | Instructions interpreted by micro-programming                                                     |
| 7    | Fixed format instructions                                        | Variable format instructions                                                                      |
| 8    | Few instructions and modes                                       | Large instructions and modes                                                                      |
| 9    | High speed of execution, without having to use memory as often   | simplify compilation, and improve overall computer performance                                    |
| 10   | attempt to reduce execution cycle by simplifying instruction set | attempt to provide a single machine instruction for each statement written in high level language |
| 11   | Hardwired control                                                | microprogrammed control                                                                           |
