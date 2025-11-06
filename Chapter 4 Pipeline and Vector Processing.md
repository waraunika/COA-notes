# 4.1  Pipelining
- technique of decomposing a sequential process into suboperations, with each subprocess being executed in a special dedicated segment that operates concurrently with all other segments.
- The overlapping of computation is made possible by associating a register with each segment in the pipeline.
- The registers provide isolation between each segment so that each can operate on distinct data simultaneously.
- Example of pipelining via the diagram and table:
  ![[pipelining_example.png]]![[pipelining_status_example.png]]
## 4.1.1 Types
- There are two major types of pipelining:
	1. Arithmetic Pipeline
		- applied to a specific, complex arithmetic operation.
		- goal is to increase the number of operations completed per second
		- e.g., floating-point multiplications
	2. Instruction Pipeline
		- applied in virtually all modern general-purpose CPU's.
		- goal is to increase the number of instructions executed per second.
		- e.g., classic RISC Pipeline (5-stage)
			1. Instruction Fetch        : IF
			2. Instruction Decode     : ID
			3. Execute                      : EX
			4. Memory Access          : MEM
			5. Write Back                 : WB
- Third type also exists: Software Pipelining
	- compiler optimisation technique, not hardware feature
	- compiler reorganises a loop's instruction so that are iterations are started before previous ones have finished.
	- compiler is manually creating a pipeline for a tight loop in the code to minimise stalls
	- goal is to keep the hardware's instruction pipeline full
## 4.1.2 Performance conditions
Using classic 5-stage RISC pipeline:
- Without pipelining, we get:
	- Instruction 1, 2, 3 and each comprises of same 5 cycles. so:
	- IF -> ID -> EX -> MEM -> WB (5 cycles, happens 3 times)
	- we get 15 cycles for 3 instructions.
- With pipelining:
	- Cycle:          1        2        3       4             5          6            7
			Instr 1        IF       ID      EX     MEM      WB     
			Instr 2                 IF       ID        EX       MEM     WB
			Instr 3                           IF         ID         EX      MEM       WB
	- So, we observe that, it only takes 7 cycles. making it 2x faster.
	- Once the pipeline is full, we complete 1 instruction per cycle
		- instead of 1 instruction per 5 cycles
- Key performance benefits:
	1. Increased Throughput
		- Non-pipelined: 1 instruction / 5 cycles = 0.2 instructions/cycle
		- Pipelined: 1 instruction / 1 instructions/cycle
		- 5x throughput improvement
	2. Better Hardware Utilisation
		- In non-pipelined design, every unit (fetch, execute, ...) is busy only 20% of the time.
		- In pipelined, except for the starting few cycles, all units are busy nearly 100% of the time.
	3. Overlap of Operations
		- While one instruction is executing, the next is decoding, and the one after that is fetching.
		- This increases parallel processing capabilities and thus speeds up the processing.
# 4.2 Parallel Processing
## 4.2.A Intro
- large class of techniques that are used to provide simultaneous data-processing tasks for the purpose of increasing the computational speed of a computer system.
- The purpose is to speed up the computer processing capability and increase its throughput.
- amount of hardware increases with increase in parallel processing power, and so does the cost.
- various levels of complexity:
	- At lowest level, type of registers used (e.g., shift registers and registers with parallel load) can distinguish between parallel and serial operations
	- At a higher level, it can be achieved by having a multiplicity of functional units that perform identical or different operations simultaneously.
- A multi-functional organisation is usually associated with a complex control unit to coordinate all the activities among the various components.
- Example: 
  ![[multifunctional_units.png]]
- Variety of ways to classify parallel processing
	- Internal organisation of the processors
	- Interconnection structure between processors
	- The flow of information through the system
## 4.2B Flynn's Classification
M. J. Flynn considers the organisation of a computer system by the number of streams of instructions and data items that are manipulated simultaneously.
- Single instruction stream, single data stream (SISD)
- Single instruction stream, multiple data stream (SIMD)
- Multiple instruction stream, single data stream (MISD)
- Multiple instruction stream, multiple data stream (MIMD)

1. **SISD**
	- represents the organisation of a single computer containing a control unit, a processor unit, and a memory unit
	- instructions are executed sequentially and the system may or may not have internal parallel processing capabilities
	- parallel processing may be achieved by means of multiple functional units or by pipeline processing
2.  **SIMD**
	- Represents an organisation that includes many processing units under the supervision of a common control unit.
	- all processors receive same instruction from the control unit, 
		- but operate on different items of data
	- the shared memory unit must contain multiple modules
		- so that it can communicate with all the processors simultaneously
3. **MISD** and 4. **MIMD**
	- MISD structure is only of theoretical interest since no practical system has been constructed using this organisation
	- MIMD organisation refers to a computer system capable of processing several programs at the same time. e.g., multiprocessor and multicomputer system.

- Flynn's classification depends on the distinction between the performance of the control unit and the data-processing unit
- emphasises the behavioural characteristics of the computer system rather than its operational and structural interconnections

- One type of parallel processing that does not fit Flynn's classification is pipelining.
- We consider parallel processing under the following main topics:
	1. Pipeline processing
		- implementation technique where arithmetic suboperations or the phases of a computer instruction cycle overlap in execution
	2. Vector processing
		- deals with computations involving large vectors and matrices
	3. Array processing
		- performs computations on large arrays of data
## 4.2C Example of parallel processing for 4 segments pipelining having 6 tasks
1. Pipeline segments:
	- Segment 1 (S1): Fetch / Input
	- Segment 2 (S2): Decode / Process
	- Segment 3 (S3): Execute / Compute
	- Segment 4 (S4): Write-back/Output
2. Task Set
	- Tasks: T1, T2, T3, T4, T5, T6
	- Each task requires processing through all 4 segments
3. Complete Time-Space Diagram:
		Time  →      |  1  |  2  |  3 |  4  |  5 |  6  |   7  |  8  |  9  |
		Segment     | ------------------------------------- |
			 S1      | T1 | T2 | T3 | T4 | T5 | T6 |      |      |      |
			 S2      |      | T1 | T2 | T3 | T4 | T5 | T6 |      |      |
			 S3      |      |      | T1 | T2 | T3 | T4 | T5 | T6 |      |
			 S4      |      |      |      | T1 | T2 | T3 | T4 | T5 | T6 |
4. Parallelism Progression
	 Time      Active Segments       Parallel Tasks       Parallelism Level
		 1                    S1                           T1                            1
		 2                 S1, S2                     T1, T2                         2
		 3              S1, S2, S3               T1, T2, T3                      3
		 4           All 4 segments        T1, T2, T3, T4              4 (Max)
		 5           All 4 segments        T2, T3, T4, T5              4 (Max)
		 6           All 4 segments        T3, T4, T5, T6              4 (Max)
		 7              S2, S3, S4               T4, T5, T6                      3
		 8                S3, S4                     T5, T6                         2
		 9                   S4                           T6                            1
5. Performance Analysis
	 1. Execution Time Comparison:
		 - Without pipelining
			 - Total time: 6 * 4 = 24 units
			 - Throughput: 6/24 = 0.25 tasks/unit
		 - With pipelining
			 - total time:   9 units
			 - throughput: 6/9 = 0.66 tasks/unit
			 - total time is improved by 2.67 times, throughput by 2.67
	2. Speedup calculations:
		- Ideal speedup = number of stages = 4
		- actual speedup = 24/ 9 = 2.67
		- efficiency = actual / ideal = 2.667 / 4 = 66.66%
6. Parallelism:
	1. Types:
		- Spatial Parallelism: Different hardware segments operate simultaneously
		- Temporal Parallelism: Multiple tasks progress through pipeline over time
	2. Parallel Processing Characteristics
		- Maximum Parallelism: Pipeline depth (4 parallel tasks)
		- Sustained Parallelism occurs during full utilisation phase
		- overhead from filling/draining phases reduces ideal speedup
7. Finally,
	- Pipelining effectively achieves parallel processing by enabling multiple tasks to utilise different pipeline segments simultaneously.
	- The 4-segment pipeline with 6 tasks demonstrates, in ideal case scenario:
		- 2.67 x performance improvement over sequential execution
		- Maximum of 4 parallel tasks processed simultaneously
		- 66.66% pipeline efficiency despite filling/draining overhead when not completely used
	- pipelining is powerful technique:
		- for using instruction-level parallelism in modern processors.
# 4.3 Arithmetic Pipeline
- usually found in very high speed computers
	- floating-point operations, multiplication of fixed-point numbers and similar computations in scientific problem
- floating-point operations are easily decomposed into sub operations
- e.g.:
	- the inputs to the floating-point adder pipeline are two normalised floating-point binary number
	  X = A x 2$^a$
	  Y = B x 2$^b$
	- A and B are two fractions that represent the mantissas
	- a and b are the exponents
- floating point and subtraction can be performed in four segments:
  ![[floating_point_addition.png]]
- the sub-operations that are performed are:
	1. Compare the exponents
		- The larger exponent is chosen as the exponent of the result
	2. Align the mantissas
		- the exponent difference determines how many times the mantissa associated with the smaller exponent must be shifted to the right.
	3. Add or subtract the mantissas
	4. Normalise the result
* When an overflow occurs, 
	* the mantissa of the sum or difference is shifted right 
	* and exponent incremented by 1
* if an underflow occurs,
	* the number of leading zeros in mantissa determines the number of left shifts in mantissa
	* and the number that must be subtracted from the exponent.
* example:
	* the comparator, shift, adder, subtractor, incrementer and decrementer in the floating-point pipeline are implemented with combinational circuit.
	* suppose that the time delays of the four segments are:
		* t1 = 60ns
		* t2 = 70ns
		* t3 = 100ns
		* t4 = 80ns
		* interface register delay, tr = 10ns
	* then,
		* pipeline floating point arithmetic delay: tp = t3 + tr = 110ns
		* non-pipeline floating point arithmetic delay: tn = t1 + t2 + t3 + t4 + tr = 320ns
		* speed up = 320/110 = 2.9
## 4.3A Example of Floating Point ADD/SUB
* ***brief explanation** given as:
	1. Compare exponents
		- purpose: determine which operand has the larger exponent
		- action: subtract exponents to find the difference
		- result: the larger exponent becomes the result's preliminary exponent
	2. Align mantissas
		- purpose: make binary points align for proper addition
		- action: shift the mantissa with smaller exponent right by |exponent difference|
		- result: both mantissas now have same exponent value
	3. Add/subtract mantissas
		- purpose: perform the actual arithmetic operation
		- action: add/subtract the aligned mantissas
		- result: raw sum/difference mantissa
	4. Normalise result
		- purpose: ensure result is in froper scientific notation form (1.xxx...)
		- action: shift mantissa left/right and adjust exponent accordingly
	- result: final normalised floating-point number
* ***detailed example*** given as: add A = 1.1101 x 2$^3$ and B = 1.001 x 2$^1$
	1. Compare exponents (CMP)
		- A: exponent = 3, mantissa = 1.101
		- B: exponent = 1, mantissa = 1.001
		- exponent difference = 3 - 1 = 2
		- larger exponent = 3 (becomes result's preliminary exponent)
	2. Align mantissas (ALM)
		- since B has smaller exponent (i), shift its mantissa right by 2 positions:
		- original B mantissa = 1.001
		- after right shift by 2: 0.01001
		- both number now have effective exponent as 3
			- A = 1.1101 x 2$^3$
			- B = 0.01001 x 2$^3$
	3. Add mantissas (ADM)
		- Add the aligned mantissas:
		- 1.10100 + 0.011001 = 1.11101
		- raw result = 1.11101 x 2$^3$
	4. Normalise result (NMR)
		- check if mantissa is in form 1.xxx...
		- current: 1.11101 x 2$^3$ -> already normalised
		- final result: 1.1101 x 2$^3$
- ***pipelined form*** can be given as:
	- can be stated as 4 stage pipeline:
		- Time:              T1       T2        T3       T4        T5       T6        T7
		- Operation:    
		- A + B            CMP    ALM     ADM    NMR
		- D - E                         CMP     ALM    ADM    NMR
		- C - E                                      CMP    ALM    ADM    NMR
		- B + E                                                  CMP    ALM    ADM    NMR
	- greater efficiency, performance ...
# 4.4 Instruction Pipeline
## 4.4.1 Concept
- technique  that overlaps the execution of multiple instructions by dividing the instruction processing into several sequential stages
- instead of processing one instruction completely before starting the next,
	- multiple instructions are in different stages of execution simultaneously
## 4.4.2 Need
- without pipelining, we get sequential execution
	- instruction execution : [fetch] -> [decode] -> [execute] -> [store]
	- only one stage active at a time
	- poor hardware utilisation
	- low throughput
- with pipelining, we get overlapped execution
	- we get time-space diagram as:
		- time:        1               2               3            4
		- inst 1:    Fetch    Decode    Execute    Store
		- inst 2:                 Fetch       Decode     Execute    Store
		- inst 3:                                 Fetch         Decode    Execute    Store
	- multiple stages active simultaneously
	- better hardware utilisation
	- higher throughput
## 4.4.3 Detailed 4-stage instruction pipeline
### Pipeline Stages:
1. Instruction Fetch (IF)
	- Fetch instruction from memory
	- Increment Program Counter (PC)
2. Instruction Decode (ID)
	- Decode instruction
	- Read register operands
	- identify instruction type
3. Execute (EX)
	- perform arithmetic/logic operation
	- calculate memory address (for load/store)
4. Write Back (WB)
	- write results to register file
	- update processor state
### Example:
1. executing these instructions:
	- ADD R1, R2, R3
	- SUB R4, R1, R5
	- LOAD R6, [100]
	- OR R7, R2, R6
2. time-space diagram:
	- Time  → |     1      |     2     |      3      |     4     |       5   |      6    |    7
	- Stage  -------------------------------------------------------
	- IF            |  ADD   |  SUB   |  LOAD  |  OR      |            |            |
	- ID            |            |  ADD   |  SUB    |  LOAD  |  OR     |            |
	- EX           |            |            |  ADD    |  SUB    |  LOAD |  OR     |
	- WB          |            |            |              |  ADD   |  SUB   |  LOAD |  OR
3. Detailed execution
	1. Time 1: 
		1. IF: Fetch ADD instruction
	2. Time 2:
		1. IF: Fetch SUB instruction
		2. ID: Decode ADD, read R2, R3
	3. Time 3:
		1. IF: Fetch LOAD instruction
		2. ID: Decode SUB, read R1, R5
		3. EX: Execute ADD (R2 + R3)
	4. Time 4:
		1. IF: Fetch OR instruction
		2. ID: Decode LOAD, calculate address
		3. EX: execute SUB (R1 - R5)
		4. WB: Write ADD result to R1
	5. Time 5:
		1. ID: Decode OR, read R2, R6
		2. EX: Execute LOAD (read from memory)
		3. WB: Write SUB result to R4
	6. Time 6:
		1. EX: Execute OR (R2 OR R6)
		2. WB: Write LOAD result to R6
	7. Time 7:
		1. WB: Write OR result to R7
### Speedup Factor calculation
1. Assumptions:
	- k = number of pipeline stages = 4
	- n = number of instructions = 4
	- each stage takes 1 time unit
2. Without pipelining:
	- time = n x k = 4 x 4 = 16 time units
3. With pipelining:
	- time = k + (n - 1) = 4 + (4-1) = 7 time units
4. Speedup factor
	- speedup = time without pipeline/ time with pipeline
	- = (n x k) / (k + n - 1)
	- = 16/7 
	- = 2.29
5. For large n (n -> $\infty$)
	- speedup = $\lim (n \rightarrow \infty)\dfrac{n \cdot k}{k + n - 1}$
- Therefore, the maximum speedup factor equals the number of pipeline stages, assuming that n is very large.
### Key points
1. Important formula:
	- pipeline time = k + n - 1
	- speedup = (n x k) / (k + n - 1)
	- maximum speedup = k
	- efficiency = speedup/k
2. Performance Limitation
	- ideal speedup = number of stages
	- actual speedup < ideal due to:
		- pipeline fill and drain overhead
		- hazards causing stalls
		- unbalanced stage delays
## 4.4.4 Conflicts/Hazards
### 4.4.4.1 Conflicts
- Root cause
- the situation that creates the potential for incorrect execution.
#### Types
1. Resource Conflicts
	- Multiple instructions competing for the same physical hardware resource simultaneously.
	- Further categories:
		 1. Memory Port Conflicts: Two instructions need memory access through same port in same cycle
		 2. Functional Unit Conflicts: Two instructions needing the same ALU, or multiple floating-point operations need FPU, etc
		 3. Register File Port Conflicts: Multiple instructions trying to read/write registers simultaneously or limited r/w ports in register file
	- To resolve: use separate instruction and data memory
2. Data Dependency Conflicts
	- Instructions have true data relationships that create ordering requirements
	- instruction depends on the result of a previous instruction, but result isn't available yet.
	- Sub-types:
		1. RAW (Read After Write)  - True Dependency 
		2. WAW (Write After Write) - Output Dependency
		3. WAR (Write After Read)  - Anti-dependency
	- Solutions
		1. Hardware interlocks: 
			- an interlock is a circuit that detects instructions whose source operands are destinations of instructions farther up in the pipeline. 
			* maintains the program sequence by using hardware to insert the required delays
		2. Operand forwarding:
			- uses special hardware to detect a conflict and then avoid it by routing the data through special paths between pipeline segments
				- requires additional hardware paths through multiplexers as well as the circuit that detects the conflict.
		3. Delayed Load:
			- The compiler for such computers is designed to detect a data conflict and reorder the instructions as necessary to delay the loading of the conflicting data by inserting no-operation instructions
3. Control Flow (Branch) Conflicts
	- Change in value of PC by branch or other instructions
	- uncertainty about which instruction should execute next
	- Sub-types:
		1. Branch Direction Conflict: should we go to jump address or continue linearly
		2. Branch Target Conflict: even if we know we're jumping, but where?
			- eg: JR R1   ; Jump through register address of R1, but what is the value of R1?
			- we don't know where until R1 is available
4. Timing Conflicts
	- Operations with different latencies creating synchronisation issues
	- Sub-types:
		1. Variable latency operations: instructions dependent on other instruction's results don't know when the results will be available
		2. Memory Hierarchy Timing: pipeline doesn't know how a long a loading instruction will take
5. Structural Capacity Conflicts
	- Limited buffer/queue sizes causing bottlenecks
	- Sub-types:
		1. Reorder Buffer Full: No space for new instruction
		2. Load/Store Queue Full: Memory operations backing up
		3. Issue Queue Full: No room to schedule new operation
		4. Register File Limits: All physical registers are already allocated
### 4.4.4.2 Hazards
- Manifested problem
- actual result of the conflict that causes incorrect behaviour in the pipeline.
#### Types
1. Structural Hazards:
	- Two instructions need the same hardware resource in the same cycle
	- example:
		- Cycle:             1     2     3         4        5        6         7        8
		- Instruction 1:  IF    ID    EX    MEM   WB
		- Instruction 2:        IF     ID      EX     MEM   WB
		- Instruction 3:                 IF       ID      EX     MEM   WB
		- Instruction 4:                           IF       ID      EX     MEM   WB
	- Conflict: if we have only one memory port, both Instruction 1 (MEM) and Instruction 4 (IF) need memory access in cycle 4.
	- Overcome:
		- Duplicate hardware (e.g., separate instruction and data caches)
		- Pipeline stall (insert bubble, but this reduce performance)
		- Smart scheduling to avoid conflicts
2. Data Hazards:
	- An instruction depends on the result of a previous instruction that hasn't finished yet.
	- Types:
		- RAW - Most common type
		- WAW
		- WAR
	- Example:
		- ADD R1, R2, R3    ; R1 = R2 + R3
		- SUB R4, R1, R5    ; R4 = R1 - R5  ← Needs R1 from previous instruction
	- Overcome:
		1. Forwarding (Bypassing)
			- most important technique
			- directly route the result from EX/MEM or MEM/WB pipeline register to ALU input
			- avoid waiting for the value to be written back to the register file
		2. Pipeline stalling
			- Insert bubbles or NOPs in the pipeline
			- used when forwarding cannot solve the hazard
		3. Compiler stalling
			- Reorder instructions to separate dependent instructions
3. Control Hazards
	- The pipeline doesn't know which instruction to fetch after a branch instruction.
	-  example:
		-            BEQ R1, R2, label  ; Branch if R1 == R2
		-            ADD R3, R4, R5      ; Should we execute this or jump to Label?
		- label:  SUB R6, R7, R8
	- overcome:
		1. Pipeline stall (same as above, simple but inefficient)
		2. Branch Prediction
			- Static Prediction: Always predict taken/not taken based on compiler hints
			- Dynamic Prediction: Use Branch Prediction Buffer (history of previous branches)
		3. Delayed Branch
			- Always execute the instructions after the branch
			- compiler tries to find useful work to put in the delay slot
		4. Advanced Techniques
			- Branch Target Buffer (BTB): Cache branch targets for faster jumping
			- Speculative Execution: Execute both paths, discard the wrong one.

4. Real-world processors
5. Instruction Fetch (IF)
6. Instruction Decode (ID)
7. Execute (EX)
8. Write Back (WB)

9. explain instruction pipeline with eg
10. describe 4 stage ip, with eg
11. show that the speedup factor for a pipelined processor is equal to no. of stages in pipeline.
# 4.5 RISC Pipeline
## 4.5.1 Intro
- instruction pipeline can be implemented with two or three segments
	- one segment fetches the instruction from program memory
	- the other segment executes the instruction in the ALU
	- third segment may be used to store the result of the ALU operation in a destination register
- the data transfer instructions in RISC are limited to load and store instructions
	- these instructions use register indirect addressing. they usually need three or four stages in the pipeline
	- to prevent conflicts between a memory access to fetch an instruction and to load or store an operand,
	- most machines use two separate buses with two memories
	- cache memory: operate at the same speed as the cpu clock
- one of the major adv of RISC is its ability to execute instructions at the rate of one per clock cycle
	- in effect, it is to start each instruction with each clock cycle 
		- and to pipeline the processor to achieve the goal of a single-cycle instruction execution.
	- RISC can achieve pipeline segments, requiring just one clock cycle.
## 4.5.2 RISC and pipeline: combo for efficiency
Why RISC architecture is fundamentally designed with efficient pipelining:
1. **Fixed Length instructions**
	- All instructions are the same size
	- this enables predictable fetching and continuous pc increment without stalls for length decoding
2. **Limited Addressing Mode & Simple Instructions**
	- only load/store instructions access memory
	- this eliminates structural hazards that occur when multiple stages need memory access simultaneously
3. **Load-Store Architecture**
	- uniform single -cycle execution times for (most) instructions
	- this prevents pipeline bubbles caused by variable-length 
4. **Regular Instruction Format**
	- consistent field positions across instructions allow parallel decoding and register reading
	- while hardwired control enables straightforward hazard detection and forwarding
- point being:
	- these features create balanced pipeline stages with minimal stalls, allowing RISC processors to achieve near-ideal throughput of 1 instruction per cycle, 
	- unlike CISC with its variable instructions and complex operations that frequently disrupt pipeline flow.
# 4.6 Vector Processing
# 4.7 Array Processing