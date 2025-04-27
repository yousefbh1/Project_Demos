# **LC-2K Two-Pass Assembler**

This project implements a **two-pass assembler** for the 16-bit LC-2K assembly language, written in C. It translates human-readable assembly code into machine code and generates relocation and symbol information necessary for linking and loading.

The assembler reads a text file of LC-2K assembly instructions, checks syntax and formatting, converts instructions into binary machine code, and outputs a corresponding machine code file.

---

## **Assembly and Parsing**

The assembler processes the input in **two passes**:
- **First Pass (Header Generation):**
  - Counts the number of text instructions, data words, defined global symbols, and relocatable references.
  - Detects global symbols (labels) and tracks undefined symbolic references.
  - Prepares the header information that describes the machine code file.
- **Second Pass (Instruction Translation):**
  - Converts each assembly instruction into corresponding binary machine code.
  - Resolves labels and symbolic references where possible.
  - Emits the translated machine code followed by the collected symbol and relocation information.

---

## **Key Features**

- **Instruction Parsing:**
  - Supports all LC-2K instructions: `add`, `nor`, `lw`, `sw`, `beq`, `jalr`, `halt`, `noop`, and `.fill`.
  - Parses labels, opcodes, and up to three arguments per line using custom string processing.
- **Symbol Management:**
  - Tracks labels that appear in the code.
  - Identifies unresolved symbolic addresses used in memory access (`lw`, `sw`) and `.fill` directives.
- **Relocation Handling:**
  - Detects which addresses must be relocated (i.e., adjusted after linking).
  - Separates relocatable references from normal constants.
- **Error Detection:**
  - Ensures no blank lines exist in the middle of the source file (except at the end).
  - Checks for excessively long lines.
  - Validates command-line usage and file accessibility.
- **Header Generation:**
  - Outputs a header containing:
    - Number of text instructions
    - Number of data words
    - Number of global symbols
    - Number of relocations
- **File Output:**
  - Writes the machine code translation and symbol information into a separate output file.

---

## **System Architecture**

The assembler is structured around:
- **`readAndParse()`**:
  - Reads and tokenizes each line into label, opcode, and operands.
- **`checkForBlankLinesInCode()`**:
  - Validates source code for blank lines and formatting errors.
- **Symbol Table Arrays**:
  - Arrays hold all detected symbols and generate appropriate relocation/symbol entries at the end.
- **Integration with External Printer:**
  - The actual machine code translation is delegated to an external `print_inst_machine_code()` function, ensuring modular design.

---

## **Input and Output**

- **Input**:
  - An LC-2K assembly file (`.as`) containing valid instructions and labels.
- **Output**:
  - A machine code file (`.mc`) containing:
    - Header line (text count, data count, symbol count, relocation count)
    - Binary translations of each instruction
    - Symbol table and relocation information

Please watch the video for a demonstration of these features.

_Please note that the source code is kept private, but can be shared if requested_
