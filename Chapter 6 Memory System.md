# 6.1 Microcomputer Memory
- Memory is an essential component of the microcomputer system
- it stores binary instructions and data for microcomputer
- is the place where the computer holds current programs and data that are in use

- no 1 single technology is optimal in satisfying the memory requirements for a computer system.
- memory exhibits perhaps the widest range of type, technology, organisation performance and cost of any feature of a computer system.

- the memory unit that communicates directly with the CPU is called main memory.
- device that provide backup storage are called auxiliary memory or secondary memory.
# 6.2 Characteristics of Memory Systems
They are:
1. **Location**
	- processor memory: registers inside CPU (fastest)
	- internal memory:    main memory/RAM (medium speed)
	- external memory:   storage devices like disks/ SSDs (slow but large)
2. **Capacity**
	- measured in bytes or words (8, 16, 32 bits)
3. **Unit of Transfer**
	- internal: word-sized via data bus
	- external: data blocks larger than words
	- addressable unit: smallest uniquely addressable location
4. **Access Method**
	- sequential access: linear access (tape) - slow
	- direct access: jump to area + search (disks) - medium
	- random access: constant access time (RAM) - fast
	- associative access: parallel search by content (cache) - fastest
5. **Performance**
	- access time: time taken to address a memory unit to having done valid data operation
	- (memory) cycle time: minimum time period between two operations (access time + recovery time)
	- recovery rate: rate at which data can be transferred in and out of memory unit
		- or transfer rate: data movement speed = 1/cycle time
6. **Physical Properties**
	- types: semiconductor (RAM), magnetic (disk), optical (CD)
	- volatility: data loss when power off (RAM = volatile)
	- erasable: can data be overwritten?
	- power consumption: how much power is consumed?
7. **Organisation**
	- physical arrangement of bits into words
	- not always obvious: e.g., interleaving for performance
# 6.3 Memory Hierarchy
![Memory Pyramid | 369][memory_pyramid.png] ![Memory Hierarchy | 325][memory_hierarchy.png]
- not all accumulated information is needed by the CPU at the same time
- so, more economical to use low-cost storage devices to serve as a backup for storing the information that isn't currently used by PCU
- memory unit that directly communicates with CPU is called the main memory
- devices that provide backup storage are called auxiliary memory

- memory hierarchy systemm consists of all storage deviced
	- from slow/high capacity/auxiliary memory
	- to faster/lower capacity/main memory
	- to even smaller and faster cache memory
- main memory occupies a central position
	- by being able to communicate directly with the CPU 
	- and with auxiliary memory devices via IO processor
- a special very high speed memory called cache is used to increase the speed of processing
	- by making current programs and data available to the CPU at a rapid rate

- CPU logic is usually faster than main memory access time
	- so processing speed is limited primarily by the speed of main memory
- cache is used for storing 
	- segments of programs currently being executed in CPU
	- and temporary data frequently needed in the present calculations

- as we go down in hierarchy,
	- cost per bit decreases
	- capacity of memory increases
	- access time increases
	- frequency of access of memory by processor also decreases
- the hierarchy contains
	- registers
	- L1 Cache, (then L2 Cache and then L3 cache and so on)
	- main memory
	- disk cache
	- disk
	- optical disk
	- tape
# 6.4 Internal and External memory
## 6.4.1 Internal/Main Memory
- main memory is the central memory unit of the computer system
- relatively large and fast memory to store programs and data during the computer operation.
- employ semiconductor integrated circuits
- basic element of semiconductor memory is memory cell
- has 3 functional terminals:
	- 'select' terminal: selects cell
	- 'data in' terminal: used to input data
- devices:
	- RAM
		- SRAM
		- DRAM
	- ROM
		- PROM
		- EPROM
		- EEPROM
		- Flash Memory
## 6.4.2 External Memory
- the devices that provide backup storage are called external memory or auxiliary memory.
- includes serial access types such as magnetic tapes and random type such as magnetic disks
- devices:
	- magnetic tape
	- optical disk (CD)
	- DVD
# 6.5 Cache memory principles
## 6.5.1 Principles:
1. cache memory acts as a high-speed buffer between the CPU and main memory to bridge the significant speed gap between them
2. provides memory access speeds close to the fastest memories available, but at a lower cost per bit by using a small amount of expensive, fast memory alongside larger, cheaper, slower main memory
3. for every memory access:
	1. the processor checks the cache first
	2. if data is found (a hit), it is delivered directly to CPU without main memory access
	3. if data is not in cache (a miss), entire memory block containing the requested data is fetched from main memory and stored in a cache "line"
4. each cache line includes a "tag", that identifies which specific block of main memory it currently holds
5. this design leverages the "locality of reference" principle:
	- programs tend to repeatedly access a small set of memory locations in a short time period, so once a block is loaded, future accesses to it will be fast cache hits.
6. The hit ratio (percentage of memory accesses found in cache) directly determines cache performance:
	- higher hit ratios mean better overall system speed.
7. on a cache hit, communication occurs only between processor and cache,
	- disabling the main system bus and buffers for maximum speed
8. on a cache miss, the required block is transferred from main memory into cache,
	- then delivered to the processor, utilising the system bus.
9. cache is physically positioned between the processor and main memory, interconnected via data, address and control lines.
   ![Cache between CPU and Main Memory | 500][cache.png]
   ![Cache Organisation | 500][cache_organisation.png]
## 6.5.2 Summary
1. Speed & Cost Balance
	1. provides near-fastest memory speed at lower cost of larger, slower memories
	2. small fast cache + large slow main memory = optimal price-performance
2. Operation Flow
	1. cache check first: all memory requests check cache first
	2. hit: data found in cache -> direct delivery to CPU (fast)
	3. miss: data no in cache -> fetch entire block from main memory to cache line, then deliver to CPU
3. Locality of Reference
	1. programs tend to access small memory regions repeatedly
	2. temporal locality: recently used data likely reused
	3. spatial locality: nearby data likely accessed soon
	4. cache exploits this by storing recently used blocks
4. Cache Structure (Organisation)
	1. lines/blocks: fixed-size units transferred from main memory
	2. tags: identify which main memory block the line contains
	3. organisation: physically between CPU and main memory
5. Performance Metrics
	1. hit ratio: percentage of requests found in cache
	2. higher hit ratio = better performance
	3. system bus only used on cache misses
6. Efficiency
	1. cache hits: no system bus traffic, direct CPU-cache transfer
	2. Cache misses: Block transfer utilises system bus to main memory
## 6.5.3 Operation
![Cache Operation | 650][cache_operation.png]
- CPU generates the receive address (RA) of a word to be moved (read).
- check a block containing RA is in cache
	- if present, get from cache (fast) and return
	- if not present, access and read required block from main memory to cache
	- allocate cache line for this new found block
	- load block for cache and deliver word to CPU
- cache includes tags to identify which block of main memory is in each cache slot
# 6.6 Elements of Cache design
## 6.6.1 Cache size
- size of cache to be small enough so that overall average cost per bit is close to that of main memory alone and large enough so that the overall average access time is close to that of the cache alone
- the larger the cache, the larger the number of gates involved in addressing the cache
- larger caches tend to be slightly slower than small ones
	- even when built with the same IC technology and put in the same place on chip and circuit board
- the available chip and board also limits cache size
## 6.6.2 Mapping function
- defn: the transformation of data from main memory to cache memory
- because there are fewer cache lines than main memory blocks, an algorithm is needed for mapping main memory blocks into cache lines
- there are three different types of mapping function in common use:
	- direct, associative, set associative
- all 3 include following elements as example:
	- cache size: 64 KB = 2$^{16}$ bytes
	- block size: 4 bytes = 2$^2$ bytes (transferred between cache and memory)
	- number of cache lines = 64 KB / 4 B = 16,384 lines = 2$^{14}$ lines
	- main memory: 16 MB = 2$^{24}$ bytes
	- address size for main memory: 24 bits
	- main memory blocks = 16 MB / 4 B = 4,194,304 = 2$^{22}$ blocks
### 6.6.2A Direct Mapping
- simplest technique
- maps each block of main memory into only one possible cache line
	- i.e., a given main memory block can be placed in one and only one place in cache
	- i = j modulo m
		- i = cache line number
		- j = main memory block number
		- m = number of line in cache
- address breakdown for the 24 bits:
	- Tag (8 bits)   |   Line Index (14 bits)   |   Block Offset (2 bits)
	- calculation:
		- block offset: $\log_2(4)$ = 2 bits
		- line index: $\log_2(16,384)$ = 14 bits
		- tag: 24 - (14 + 2) = 8 bits
- implementation:
	- cache structure: each line has [Tag (8 bits) + Data (4 bytes) + Valid bit]
- example:
	- memory block 0, 16,384, 32768, ... all map to cache line 0
	- memory block 1, 16,385, 32769, ... all map to cache line 1
- pros: simple hardware - just one comparator
- cons: high conflict misses - blocks compete for same line
### 6.6.2B (Fully) Associative Mapping
- any main memory block can map to any line in cache
- address breakdown for 24 bits:
	- Tag (22 bits)   |   Block Offset (2 bits)
	- calculation:
		- block offset: $\log_2(4)$ = 2 bits
		- tag: 24 - 2 = 22 bits
- implementation:
	- cache structure: each line has [Tag (22 bits) + Data (4 bytes) + Valid bit]
	- search: compare 22-bit tag with all 16,384 lines simultaneously
	- replacement: requires LRU or similar algorithm when cache is full
- pros: optimal utilisation: no conflict misses
- cons: complex hardware - 16,384 parallel comparators needed
           higher power consumption
### 6.6.2C Set Associative Mapping
- cache is divided into sets, each set contains multiple lines.
- each block maps to a specific set but can occupy any line within that set
- configuration (example: 2 line per set):
	- lines per set = 2
	- number of sets = 16,384 / 2 = 8,192 = 2$^{13}$ sets
- address breakdown:
	- Tag (9 bits)   |   Set Index (13 bits)   |   Block Offset (2 bits)
	- calculation
		- block offset: $\log_2(4)$ = 2 bits
		- set index: $\log_2(8,192)$ = 13 bits
		- tag: 24 - (13 + 2) = 9 bits
- implementation:
	- cache structure: each set has 2 lines, each with [tag (9 bits) + Data (4 bytes) + Valid bit]
	- mapping: Set = (Main memory Block Address) mod (Number of Sets)
	- search: compare tag with only 2 lines in the identified set
- pros: 
	- good compromise between direct and associative
	- reasonable hardware: only 2 comparators per access
	- fewer conflict misses than direct mapping
### 6.6.2D Comparison of examples

| Type           | Tag size | Comparators Needed | Conflict Misses | Hardware Complexity |
| -------------- | -------- | ------------------ | --------------- | ------------------- |
| Direct         | 8 bits   | 1                  | High            | Low                 |
| Associative    | 22 bits  | 16,384             | None            | Very High           |
| 2-way Set Ass. | 9 bits   | 2                  | Medium          | Medium              |
### 6.6.2E Differences of mapping functions

| Aspect             | Direct                                                             | Associative                                                            | Set-Associative                                                                 |
| ------------------ | ------------------------------------------------------------------ | ---------------------------------------------------------------------- | ------------------------------------------------------------------------------- |
| Placement          | Each memory block maps to a specific, fixed cache line             | any memory block can be placed in any cache line                       | each memory block maps to a specific set but can occupy any line within the set |
| Complexity         | Simple design with minimal control logic                           | Complex design requiring full associative searching                    | Moderately complex, as only the lines within the mapped set are searched        |
| Conflict Misses    | High due to fixed mapping, leading to frequent overwrites          | None, as any block can occupy any line in the cache                    | Low, as blocks compete only within the mapped set, reducing overwrites          |
| Hardware Cost      | Low, as minimal comparators and hardware are needed                | High, due to the need for comparators and associative lookup mechanism | Moderate, balancing the cost of partial associative searching                   |
| Tag Size           | Small, as only part of the memory address is stored for each block | Large, since the entire memory block address must be stored            | Moderate, as only the set index and partial address need to be stored           |
| Replacement Policy | Not required, as each block has a fixed cache line                 | Required, to determine which block to evict when cache is full         | Required within the set to manage cache line replacement                        |
## 6.6.3 Replacement algorithm
- when all lines are occupied, bringing a new block requires that:
	- an existing line be overwritten
- in various mappings:
	1. Direct mapping:
		1. no choice possible
		2. each block only maps to one line
		3. replace that line
	2. Associative and Set Associative mapping
		1. Algorithms must be implemented in hardware for speed
		2. Least Recently Used (LRU), First in First out (FIFO), Least frequently used (LFU), random are commonly used.
### 6.6.3A FIFO
```pseudocode
Input: Page reference sequence S, frame count n
1. Initialize empty queue Q of size n
2. Initialize hit counter h ← 0
3. for each page p in sequence S do
4.     if p ∈ Q then
5.         h ← h + 1
6.     else
7.         if |Q| ≥ n then
8.             Q.dequeue()  // Remove oldest
9.         end if
10.        Q.enqueue(p)     // Add new page
11.    end if
12. end for
13. Output: Hit count h, hit ratio = h/|S|
```
### 6.6.3B LRU
```pseudocode
Input: Page reference sequence S, frame count n
1. Initialize empty list L (size ≤ n)
2. Initialize hit counter h ← 0
3. for each page p in sequence S do
4.     if p is in L then
5.         h ← h + 1
6.         Move p to the front of L  // Most recently used
7.     else
8.         if |L| = n then
9.             Remove the last element from L  // Least recently used
10.        end if
11.        Insert p at the front of L
12.    end if
13. end for
14. Output: Hit count h, total pages |S|
```
### 6.6.3C LFU
```pseudocode
Input: Page reference sequence S, frame count n
1. Initialize empty list L (size ≤ n)
2. Initialize frequency counter F (same size as L)  
3. Initialize timestamp T (same size as L)
4. Initialize hit counter h ← 0, time ← 0
5. for each page p in sequence S do
6.     time ← time + 1
7.     if p is in L then
8.         h ← h + 1
9.         Increase frequency of p in F by 1
10.        Update timestamp of p in T to current time
11.    else
12.        if |L| = n then
13.            Find pages with smallest frequency in F
14.            If multiple, choose the one with oldest timestamp
15.            Remove the selected page from L, F, and T
16.        end if
17.        Insert p at the end of L
18.        Set frequency of p in F to 1
19.        Set timestamp of p in T to current time
20.    end if
21. end for
22. Output: Hit count h, total pages |S|
```
## 6.6.4 Write Policy
### 6.6.4A Problem
- when cache data is modified, main memory becomes outdates
- must ensure main memory reflects changes before cache line replacement
- 15% of memory references are writes -> significant performance impact
- multiple CPUs / I/O modules may need consistent memory views
### 6.6.4B Write-Through Policy
- every write goes to both cache and main memory simultaneously
- main memory is always valid and up-to-date
- simple, but it generates heavy memory traffic
- characteristics:
	- CPU Write -> [Update Cache] + [Update Main Memory]
	- both copies always agree
	- no stale data problems
- adv:
	- simple implementation
	- excellent data consistency
	- easy cache coherency in multi-processor systems
- disadv:
	- high memory traffic
	- creates potential bottlenecks
	- slower write operations
### 6.6.4C Write-Back Policy
- writes only update cache, not main memory
- each cache line has "dirty bit" (update bit)
- when line is replaced: if dirty -> write to main memory, else discard
- characteristics:
	- CPU Write -> [Update Cache + Set Dirty Bit]
	- On Replacement -> [If Dirty: Write to Main Memory]
	- main memory is temporarily outdated
	- reduced memory traffic (only dirty lines written back)
- adv:
	- much less memory traffic
	- faster write operations
	- better performance for write-intensive applications
- disadv:
	- cache coherency problems
	- complex implementation
	- i/o must go through cache to see latest data
### 6.6.4D Cache Coherency Solutions (Multi-processor)
- multiple caches can have different values for same memory address
- this can lead to potential conflicts in short term as well as long term due to data inconsistency
- solutions:
	1. Bus Watching with Write-Through
		- All caches use write-through
		- caches monitor memory bus for writes by other processors
		- if another cache writes an address we have invalidate
	2. Hardware Transparency
		- direct hardware links between caches
		- write to one cache automatically updates other caches
		- most consistent but expensive
	3. Non-Cacheable Memory
		- shared memory regions marked as non-cacheable
		- forces all accesses to go directly to main memory
		- simple but slower for shared data
- performance comparison between write-through vs write-back:
	- memory traffic:  high       vs   low
	- write speed:        slow       vs   fast
	- complexity:          low        vs   high
	- coherency:           easy      vs   difficult
	- i/o handling:        simple  vs  complex
## 6.6.5 Number of caches
