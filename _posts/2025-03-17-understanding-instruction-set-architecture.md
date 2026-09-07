---
layout: post
title: "Understanding Instruction Set Architecture (ISA)"
tags: [embedded, cpu-architecture, isa, toolchains]
---

*Originally published on [Medium](https://medium.com/@shravansingh64/understanding-instruction-set-architecture-isa-4e45f2874762).*

The goal here is to understand ISAs and why they matter for embedded
systems.

An ISA is defined as the "attributes of a computing system as seen by
the programmer" — the design of a computer from the programmer's
perspective. In other words, the ISA describes a computer in terms of
the basic operations it must support. We can think of the ISA as the
"language" used to communicate with the CPU.

There are two main families of ISAs:

1. **Complex Instruction Set Computer (CISC):** a single instruction
   can perform more than one underlying operation in the CPU.
   Different CISC instructions have different lengths. The downside is
   a much more complex decoder.
2. **Reduced Instruction Set Computer (RISC):** instructions have a
   fixed length. For example, in RISC-V the destination register is
   always encoded in the same position, and the source operands are
   always in the same positions — faster and easier to decode and
   implement. If you look at the RISC-V encoding of the `add`
   instruction, 5 bits encode one register, which implies a maximum of
   2^5 = 32 registers as part of the ISA.

Let's walk through the objectives of an ISA using MIPS as the example.
MIPS is the classic teaching ISA — clean, fixed-length, and heavily
used in textbooks — which makes it ideal for this. (Today's embedded
world is dominated by ARM, with RISC-V growing fast; the concepts
transfer directly.)

MIPS — Microprocessor without Interlocked Pipelined Stages — is a
family of RISC instruction set architectures.

The objectives of an ISA are as follows.

**A. The ISA defines the types of instructions the processor
supports.** MIPS instructions fall into three classes:

1. **Arithmetic/logic instructions** — perform arithmetic and logic
   operations.
2. **Data transfer instructions** — move data between memory and the
   processor.
3. **Branch and jump instructions** — necessary to implement function
   calls and conditional statements.

**B. The ISA defines the maximum length of each instruction.** MIPS is
32-bit, so each instruction must fit within 32 bits.

**C. The ISA defines the instruction format of each type.** The format
determines how the entire instruction is encoded within those 32 bits.
MIPS has three:

1. **R format** — all source operands are registers.
2. **I format** — one operand is a constant encoded in the instruction
   itself (an immediate).
3. **J format** — instructions that change the flow of execution
   (J for jump), used to implement control logic.

![Abstraction hierarchy from ISA down to microarchitecture](/assets/images/isa-abstraction-hierarchy.png)

**What is microarchitecture?** It is the implementation of the basic
operations defined by the ISA. That's how an AMD chip and an Intel
Core 2 Duo can be based on the same ISA yet have completely different
microarchitectures.

Intel developed the x86 architecture, ARM developed the ARM
architecture, and AMD developed amd64. RISC-V, developed at UC
Berkeley, is an example of an open-source ISA.

From an embedded systems point of view, the compiler/cross-compiler
needs to understand the ISA: what data types are available, how many
registers, what kinds of registers, and so on.

## Embedded targets

![Embedded target triplets and toolchain components](/assets/images/isa-embedded-targets.png)

**GNU** is an extensive collection of free software that can be used as
an operating system or in parts alongside other operating systems. Its
C library, **glibc**, provides a wrapper around the system calls of the
Linux kernel (and others) for applications to use, and it also supports
C++. It is the de facto C library for most Linux distributions.

**musl** was created in response to the increasing complexity and size
of glibc. It is a C standard library designed for efficient static
linking and real-time-quality robustness — avoiding race conditions,
internal failures on resource exhaustion, and various other bad
worst-case behaviors present in existing implementations.

(A statically linked library contains functions and data included in
the consuming program at build time, so it doesn't need to be
accessible as a separate file at run time. If all libraries are
statically linked, the resulting executable is fully standalone.)

**ABI** — the application binary interface — is the interface between
two binary program modules, where often one of the modules is a library
or the operating system. The ABI defines how data structures and
routines are accessed in machine-level, hardware-dependent format,
whereas an API defines that access at the source-code level. The ABI
includes how data types are laid out in memory, how nested function
calls work, and how program startup/initialization works.

**HF (hard float)** indicates that the compiler and its underlying
libraries use hardware floating-point instructions.

## References

1. [Instruction set architecture — Wikipedia](https://en.wikipedia.org/wiki/Instruction_set_architecture)
2. [Microarchitecture and ISA — GeeksforGeeks](https://www.geeksforgeeks.org/microarchitecture-and-instruction-set-architecture/)
3. [Application binary interface — Wikipedia](https://en.wikipedia.org/wiki/Application_binary_interface)
4. [Static library — Wikipedia](https://en.wikipedia.org/wiki/Static_library)
5. [musl — Wikipedia](https://en.wikipedia.org/wiki/Musl)
6. [glibc — Wikipedia](https://en.wikipedia.org/wiki/Glibc)
7. [GNU — Wikipedia](https://en.wikipedia.org/wiki/GNU)
