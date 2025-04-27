
# **LC-2K Pipelined Processor Simulator**

This project implements a user-level simulator for a **5-stage pipelined LC-2K processor**, written in C. It models the classic stages of a CPU pipeline: **Instruction Fetch (IF)**, **Instruction Decode (ID)**, **Execute (EX)**, **Memory Access (MEM)**, and **Write Back (WB)**. The simulator provides an educational visualization of pipelined execution, handling control hazards, data forwarding, stalls, and memory operations at the instruction level.

**Instruction Pipeline Management**

The core simulation loop models how instructions move through the 5 pipeline stages on each cycle. It also simulates key pipeline hazards and forwarding mechanisms.

**Key Features:**
- **Instruction Fetch:** Fetches the next instruction from instruction memory using the program counter (PC).
- **Decode Stage:** Parses the fetched instruction, reads source registers, and calculates immediate values.
- **Execution Stage:** Performs arithmetic, logic, and branch target computations while forwarding values from later pipeline stages if necessary to avoid stalls.
- **Memory Access Stage:** Loads from and stores to the data memory as needed by LW and SW instructions.
- **Write-Back Stage:** Writes computed or loaded values back into the register file for ADD, NOR, and LW instructions.
- **Stalling and Forwarding:** Implements detection of load-use hazards requiring a stall, as well as forwarding from MEM and WB stages to EX to reduce stalls.

**Processor State**

The processor's entire state is modeled through a `stateType` structure.

**State includes:**
- **PC register**: Tracks the current program counter.
- **Register file**: 8 general-purpose registers.
- **Instruction and Data Memory**: Modeled as separate arrays (both 65,536 words).
- **Pipeline Registers**: Separate structures (`IFID`, `IDEX`, `EXMEM`, `MEMWB`, `WBEND`) simulate the data passed between pipeline stages each cycle.
- **Cycle Counter**: Tracks the number of completed clock cycles.

**Hazard Handling**

To simulate realistic pipelined execution:
- **Load-Use Stall Detection**: If a LW is immediately followed by an instruction using the loaded register, the pipeline inserts a stall (NOOP) into the decode stage.
- **Data Forwarding**: Implements simple forwarding paths to avoid stalls when source operands are produced by instructions in later stages (EX/MEM/WB).

**Instruction Set**

The simulator supports the following LC-2K instructions:
- **ADD, NOR:** Arithmetic and bitwise operations.
- **LW, SW:** Load and store operations with offsets.
- **BEQ:** Conditional branch if two registers are equal.
- **HALT, NOOP:** Halt execution or insert no-operations.

(Instructions like JALR are parsed but not implemented for this simulation.)

**Memory and I/O**

- Instructions and data are read from a text file containing machine mode at startup.
- Output shows detailed per-cycle state, including PC, register contents, pipeline registers, and memory.

**Execution Model**

The simulator continuously cycles until a HALT instruction reaches the Write-Back (WBEND) stage. Each cycle, the simulator:
- Advances instructions through pipeline registers.
- Applies data forwarding if needed.
- Introduces stalls if necessary.
- Updates the machine state for the next cycle.

Please watch the video for a demonstration of these features.

_Please note that the source code is kept private, but can be shared if requested_
