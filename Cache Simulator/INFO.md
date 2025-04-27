# **LC-2K Cache Simulator**

This project implements a **fully associative cache simulator** for a 16-bit LC-2K architecture, written in C. It models realistic cache behavior, including memory block management, Least Recently Used (LRU) eviction policies, dirty block handling, and interactions with main memory through controlled read and write operations.

The simulator provides insight into how hardware caches operate between processors and memory, focusing on minimizing memory latency and optimizing memory access patterns.

---

## **Cache Management and Organization**

The simulator models a configurable cache system, parameterized by:
- **Block Size** (words per block)
- **Number of Sets** (indexed by address bits)
- **Blocks per Set** (lines per set)

Each memory access is processed through the cache logic:
- Cache **read** (load word) and **write** (store word) operations.
- Handling **cache hits** and **cache misses**.
- **Write-back policy** for dirty blocks and **evictions**.
- **LRU replacement policy** within each set.

---

## **Key Features**

- **Flexible Cache Initialization:** Supports arbitrary block sizes, set counts, and associativity (subject to maximum constraints).
- **Address Parsing:** Efficiently extracts **index**, **tag**, and **offset** bits from 16-bit memory addresses.
- **Miss Handling:** 
  - **Evicts** least recently used blocks when sets are full.
  - **Handles dirty blocks** by writing modified data back to memory before replacement.
- **LRU Replacement:** 
  - Tracks usage with per-block `lruLabel`.
  - Updates LRU labels dynamically after every access.
- **Memory Transfers:**
  - **memory → cache** on misses.
  - **cache → processor** on reads.
  - **processor → cache** on writes.
  - **cache → memory** when dirty data is evicted.
- **Statistics Tracking:**
  - Counts and reports **cache hits**, **misses**, and **write-backs** at the end of execution.

---

## **System Architecture**

The main cache structure (`cacheStruct`) consists of:
- **Block Arrays**: Each block (`blockStruct`) stores:
  - Data array (words per block).
  - Dirty bit (whether data was modified).
  - Tag for block identification.
  - LRU label for replacement decisions.
- **Global Metadata**:
  - Cache size, associativity, offset/index bit counts, and operation counters.

All cache operations are processed through `cache_access()`, including proper synchronization with backing memory (`mem_access()`).

---

## **Cache Policies Implemented**

| Feature             | Implementation         |
| --------------------|-------------------------|
| Block Placement      | Set associative (configurable) |
| Replacement Policy    | Least Recently Used (LRU) |
| Write Policy          | Write-back on eviction |
| Allocation Policy     | Load entire block on miss |

---

## **Memory Interface**

All memory interactions are abstracted via:
- `mem_access(addr, write_flag, write_data)`: Performs load or store.
- `get_num_mem_accesses()`: Returns the total number of memory accesses performed.

Cache minimizes memory accesses by satisfying as many requests as possible internally, deferring memory updates until dirty evictions.

---

## **Action Logging**

Throughout execution, cache behavior is logged with:
- **Cache to Processor**
- **Processor to Cache**
- **Memory to Cache**
- **Cache to Memory**
- **Cache to Nowhere**

using the `printAction()` function to ensure detailed tracking of every cache interaction.

---

## **End-of-Run Statistics**

Upon halt, the simulator optionally prints:
- Total **cache hits**
- Total **cache misses**
- Total **write-backs** to memory

to help assess cache performance for a given memory access pattern.

---

## **Important Notes**
- Input parameters are validated for correctness (e.g., positive, power of two).
- Cache and memory arrays are statically allocated for simplicity and performance.
- Provided debugging utilities like `printCache()` are available to inspect the internal cache state during development.

Please watch the video for a demonstration of these features.

_Please note that the source code is kept private, but can be shared if requested_
