Virtual Memory Pager
_Built using: C++_

This project implements a virtual memory management system that simulates key features of an operating system pager. It handles virtual-to-physical memory translation, demand paging, copy-on-write behavior, and eviction using a clock-based replacement algorithm.

The pager supports multiple processes, swap-backed and file-backed memory, and simulates low-level system behaviors like page faults and dirty page handling.

**Features**
- Virtual Memory Mapping
  - Supports both swap-backed (anonymous) and file-backed memory.
  - Pages are loaded on demand when first accessed.

- Page Fault Handling
  - Handles read and write faults, allocating physical memory as needed.
  - Implements copy-on-write to avoid unwanted sharing when processes write to shared pages.

- Page Replacement
  - Uses a simplified clock algorithm to evict pages when physical memory is full.
  - Handles dirty and referenced bits, writing dirty pages back to disk when evicted.

- Process Isolation
  - Each process has its own page table and virtual memory space.
  - Proper cleanup of page tables and physical pages on process destruction.

- Swap and File I/O
  - Integrates with file_read and file_write to simulate loading pages from a file.
  - Tracks swap blocks using a bitmap and supports writing dirty pages to swap when evicted.

Please watch the video for a demonstration of these features.

_Please note that the source code is kept private, but can be shared if requested_
