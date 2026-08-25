---
layout: post
title: "Reverse engineering roadmap"
date: 2026-08-23
author: pokyuser
tags: RE, binary, GDB, Ghidra, cracking, rootme, reverse
---

> If you can read assembly language then everything is open source.
>
><cite>— Unknown; quoted in *Learning Linux Binary Analysis*, Ryan “elfmaster” O'Neill</cite>

Here is my roadmap to improve reverse engineering skills.
Some basic knowledge of C, assembly and Operating System is assumed.

The roadmap is divided into 2 parts :
1. Understanding executable file format and the first steps in reverse engineering
2. Learning methods and software

Part 1 is a collection of foundations and references. Part 2 is the actual learning path.

## Part 1 : Understanding executable file format and the first steps in reverse engineering

- Warm-up: [Caichinger - elf](https://www.caichinger.com/elf)

- Warm-up (French): [Univ-Paris](https://sysblog.informatique.univ-paris-diderot.fr/2024/04/01/le-format-elf-executable-and-linkable-format/)

- Toolchain, executable autopsy (French): 
<details>
<summary>Telecom-Paris - Chaîne de compilation, Genèse et autopsie des exécutables</summary>    
ⓒ 2020 Alexis Polti  
ⓒ 2021-2024 Samuel Tardieu  
</details>

- Language C, Toolchain and machine-level: [NYU - A.Gottlieb](https://cs.nyu.edu/~gottlieb/courses/cso/class-notes.html)

- Design and implementation of compilers: [Cornell - A.Myers](https://www.cs.cornell.edu/courses/cs4120/2026sp/notes/)

- Introduction to reverse and some methods: <em>Learning linux binary analysis</em> - Ryan "elfmaster" O'Neil

- Additional information: [Oracle](https://docs.oracle.com/cd/E19455-01/816-0559/6m71o2afp/index.html) : chapter 8 about map file is slightly interesting

- ELF Reference: [TIS Committee](https://cs.nyu.edu/~mwalfish/classes/15fa/ref/elf-1.2.pdf)

- Practical training: [pwn.college](https://pwn.college/welcome/welcome/)

If you are missing some knowledge or you want to dig deep into a specific subject you still can use meta keyword while searching : 

> site:edu <em>subject</em>

## Part 2 : Learning methods and software

Although the phases are numbered, starting from phase #2 you can start mixing phases.  
Alternate theory and practice to deepen your understanding.

### PHASE 1 — OS
1. [OSTEP](https://pages.cs.wisc.edu/~remzi/OSTEP/)
    - Processes
    - Process API
    - Address Spaces
    - Virtual Memory


2. [GDB](https://sourceware.org/gdb/documentation/)
    - Learn some commands
    - Try on small program you wrote
    - Exercises : attach and dynamic analysis



### PHASE 2 — UNDERSTAND COMPILED CODE
3. [Reverse Engineering for Beginners](https://beginners.re/)

4. [CS:APP + Bomb Lab](https://csapp.cs.cmu.edu/3e/)
    - Compiler idioms
    - Machine code
    - Analysis without source code


### PHASE 3 — REVERSE TOOLS
5. [Ghidra](https://ghidra.re/ghidra_docs/)
    - Beginner
    - Intermediate
    - Ghidra + GDB


### PHASE 4 — PRACTICAL REVERSE
6. [Root-Me](https://www.root-me.org/)
    - The first cracking challenges are quite easy


7. [pwn.college - Cyber / RE](https://pwn.college/)
    - [Cybersecurity - RE](https://pwn.college/intro-to-cybersecurity/reverse-engineering)
    - [Reversing Hell](https://pwn.college/reversing-hell/)


### PHASE 5 — DYNAMIC ANALYSIS
8. [Frida](https://frida.re/docs/)
    - Instrumentation
    - Complete GDB


### PHASE 6 — COMPLEX BINARIES
9. Practical Reverse Engineering
10. [OST2 — C++ Reverse Engineering](https://p.ost2.fyi/)


### PHASE 7 — BINARY ANALYSIS
11. [Practical Binary Analysis](https://nostarch.com/binaryanalysis)
    - CFG
    - Data-flow
    - Slicing
    - Instrumentation
    - Taint


### PHASE 8 — ADVANCED TECHNIQUES
12. [Symbolic execution](https://docs.angr.io/)
    - angr
    - OST2 RE3201

13. Obfuscation / anti-analysis
    - packing
    - anti-debugging
    - control-flow flattening
    - self-modifying code
    - VM obfuscation


### PHASE 9 — BROADENING
14. Choose one or several branches:
    - ARM64
    - Windows / PE
    - Go
    - Rust


### PHASE 10 — FREE Exercises
15. [Crackmes](https://crackmes.one/)

16. [Root-Me](https://www.root-me.org/)

17. [FLARE-ON](https://www.flare-on.com/)


### PHASE 11 — PROJECTS
18. Reverse real binaries :
    - C/C++ stripped + optimized
    - ARM64
    - PE
    - Go/Rust
    - obfuscated binaries
    - advanced FLARE-ON and Root-Me challenges
