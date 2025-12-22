# Computer Systems Foundations [(CMU 15-213)](https://www.youtube.com/watch?v=cEFXuhPm65k&list=PLtv4PQFH2yMas_SCMTEaE4eUIWWWiwISj)
### Understanding the Hardware-Software Interface

## Overview
This repository is dedicated to my study of **CMU 15-213: Introduction to Computer Systems (CSAPP)**. The goal is to master how computer systems execute programs, store information, and communicate. 

For a student with a background in statistics, this course provides the essential hardware-level context required to optimize high-performance data processing and understand memory-level constraints in large-scale computing.



## Learning Objectives
* **Data Representation:** Understanding how integers and floating-point numbers are represented at the bit level and the implications for numerical precision in statistical computing.
* **Memory Hierarchy:** Analyzing the impact of L1/L2/L3 caches on program performance (locality).
* **Virtual Memory:** Mastering how the system manages memory and protects processes.
* **System Interaction:** Learning how application programs interact with the operating system and hardware.

## Lab Implementations (Progress)
* [ ] **Data Lab:** Manipulating bits to implement arithmetic and logical operators.
* [ ] **Bomb Lab:** Defusing a binary bomb by tracing assembly code and understanding the stack.
* [ ] **Attack Lab:** Understanding system security by exploiting buffer overflows.
* [ ] **Cache Lab:** Building a cache simulator and optimizing matrix memory access.
* [ ] **Shell Lab:** Implementing a tiny Unix shell to understand process control and signals.
* [ ] **Malloc Lab:** Writing a custom dynamic memory allocator to understand heap management.



## Technical Notes
Weekly deep-dives and summaries of core concepts:
* [01. Data Representation & Bits](./notes/01-data-bits.md)
* [02. Assembly Language & Machine Level Program](./notes/02-assembly.md)
* [03. The Memory Hierarchy & Cache](./notes/03-memory-hierarchy.md)
* [04. Exceptional Control Flow (Processes & Signals)](./notes/04-ecf.md)
* [05. Virtual Memory & Garbage Collection](./notes/05-virtual-memory.md)

## Tech Stack
* **Language:** C (GNU99)
* **Tools:** GDB (Debugger), Objdump, Valgrind, Make
* **Environment:** Linux (Ubuntu/WSL2)
