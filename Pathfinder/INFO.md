# **MST and TSP Solver for LC-2K**

This project implements a command-line solver for **graph optimization problems** in C++, specifically focusing on:
- **Minimum Spanning Tree (MST)** computation using Prim’s Algorithm,
- **Approximate Traveling Salesman Problem (FASTTSP)** solution via greedy nearest-neighbor + 2-opt optimization,
- **Exact Traveling Salesman Problem (OPTTSP)** solution using branch-and-bound with MST-based pruning.

The solver reads a set of 2D points from standard input and applies different optimization strategies based on the specified operating mode.

---

## **Modes of Operation**

- **MST (`--mode=MST`)**: 
  - Computes a Minimum Spanning Tree (MST) among the nodes, while respecting restrictions based on node status (Safe, Dangerous, Border).
  - Dangerous-Safe connections are forbidden and treated as infinite distance.

- **FASTTSP (`--mode=FASTTSP`)**:
  - Computes an approximate TSP solution.
  - Starts from a greedy nearest-neighbor path and applies a 2-opt heuristic to improve tour length.

- **OPTTSP (`--mode=OPTTSP`)**:
  - Computes an *exact* minimum-length TSP tour.
  - Uses a **branch-and-bound** strategy enhanced by MST estimation and outgoing/incoming arms pruning to eliminate non-promising paths.

---

## **Key Features**

- **Flexible Input Parsing**: 
  - Reads 2D coordinates and assigns a node status based on quadrant (Safe, Border, Dangerous).
- **Efficient Distance Calculations**: 
  - Supports weighted graphs with infinite edge costs when necessary.
- **Prim’s MST Algorithm**: 
  - Custom MST implementations with and without node restrictions.
- **Greedy + 2-Opt Heuristic**:
  - FASTTSP mode improves upon naive nearest neighbor tours by iteratively reversing segments (2-opt) for shorter paths.
- **Branch-and-Bound Pruning**:
  - OPTTSP mode aggressively prunes bad permutations using MST lower bounds and arm costs.
- **Command-Line Argument Parsing**: 
  - Supports options using `getopt_long` for robust CLI behavior.
- **Precise Output Formatting**:
  - Distance outputs formatted to two decimal places for clarity and grading compliance.

---

## **System Architecture**

- **Node Representation** (`Node` struct):
  - Contains 2D coordinates, status, MST information (minWeight, preceeding vertex), and visited flags.

- **Option Management** (`OptLibrary` class):
  - Parses mode selection and configures program behavior.

- **Solver Classes**:
  - **Prims / PrimsOpt**: MST computation based on connectivity rules.
  - **FastTSP**: Greedy path builder + 2-opt improvement.
  - **opt Class**: Recursive branch-and-bound generator for TSP with optimality guarantee.

---

## **Algorithmic Details**

| Feature                  | Implementation Details                        |
|---------------------------|------------------------------------------------|
| MST Computation            | Prim’s Algorithm with status-aware distances |
| Approximate TSP (FASTTSP)  | Greedy nearest neighbor + 2-opt optimization |
| Exact TSP (OPTTSP)         | Branch-and-bound with MST lower bounds       |
| Distance Metric            | Euclidean distance (sqrt at evaluation)     |
| Pruning Criteria           | Sum of arms + MST lower bound estimate      |

---

## **Important Notes**
- Status-based edge restrictions (Safe vs Dangerous) are enforced during MST construction.
- Distance calculations delay the square root for efficiency except during output or TSP total distance checks.
- Fast approximate solutions are used as starting points for full permutation search to improve OPTTSP efficiency.
- Debugging and verbose outputs are cleanly suppressed for performance, except for formatted outputs.
