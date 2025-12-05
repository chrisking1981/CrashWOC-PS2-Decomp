# Project Structure

This document describes the intended folder layout for reconstructed C code in the Crash Bandicoot: The Wrath of Cortex PS2 decompilation project.

## Directory Layout

```
CrashWOC-PS2-Decomp/
├── src/                        # Reconstructed C source files
│   ├── nu_engine/             # Nu Engine 2 functions
│   │   ├── file/              # File I/O system
│   │   ├── memory/             # Memory management
│   │   ├── math/               # Math utilities
│   │   ├── render/             # Rendering system
│   │   └── ...
│   ├── game/                   # Game-specific code
│   │   ├── crash/              # Crash-specific logic
│   │   ├── levels/             # Level code
│   │   └── ...
│   ├── lib/                    # Library wrappers
│   │   ├── sony/               # Sony SDK wrappers
│   │   └── newlib/             # Newlib C library
│   └── startup/                # Entry point and initialization
├── include/                    # Header files
│   ├── nu_engine/              # Nu Engine headers
│   ├── game/                   # Game headers
│   └── lib/                    # Library headers
├── symbols/                    # Function & variable names (exported from Ghidra)
│   ├── FUNCTIONS.txt           # All function addresses and names
│   ├── VARIABLES.txt           # All variable addresses and names
│   └── ALL_SYMBOLS.txt         # Complete symbol list
├── docs/                       # Research notes and documentation
│   ├── ADVANCED_DECOMPILE_ANALYSIS.txt
│   └── project_structure.md    # This file
├── tools/                      # Scripts for Ghidra/analysis
│   ├── ghidra_import.py        # Import symbols into Ghidra
│   └── compare.py              # Compare compiled output with original
├── ghidra/                     # Exported symbols & datatypes from Ghidra
│   ├── symbols.xml             # Ghidra symbol export
│   └── datatypes.gdt           # Ghidra datatype export
└── build/                      # Compiled objects (gitignored)
```

## PS2 Binary Structure

The original PS2 ELF binary (SLES_503.86) follows a standard structure:

### Sections

- **.text** - Executable code (functions)
- **.rodata** - Read-only data (constants, strings)
- **.data** - Initialized global variables
- **.bss** - Uninitialized global variables
- **.sdata** - Small initialized data (GP-relative)
- **.sbss** - Small uninitialized data (GP-relative)
- **.mdebug** - Debug information (MDEBUG/STABS format)

### Memory Layout

- **Entry Point**: 0x00100008
- **GP Register**: 0x00634970
- **RAM Base**: 0x00100000
- **RAM Size**: 32MB

## Source File Organization

### Nu Engine Code

The Nu Engine 2 is the proprietary game engine used by Traveller's Tales. Functions are organized by subsystem:

- **File System** (`nu_engine/file/`) - File I/O, DAT file handling
- **Memory** (`nu_engine/memory/`) - Memory allocation and management
- **Math** (`nu_engine/math/`) - Vector, matrix, and math utilities
- **Rendering** (`nu_engine/render/`) - Graphics and rendering pipeline
- **VU1** (`nu_engine/vu1/`) - Vector Unit 1 microcode

### Game Code

Game-specific code is separated from engine code:

- **Crash Logic** (`game/crash/`) - Character-specific code
- **Levels** (`game/levels/`) - Level-specific implementations
- **Game Systems** (`game/systems/`) - Game mechanics

### Library Code

Standard library wrappers and implementations:

- **Sony SDK** (`lib/sony/`) - Wrappers for PS2 SDK functions
- **Newlib** (`lib/newlib/`) - C standard library implementations

## Build System

The project will use a Makefile-based build system that:

1. Compiles individual `.c` files to `.o` objects
2. Links objects into a final ELF binary
3. Compares the output with the original binary for bit-matching

### Compiler Flags

Based on analysis, the original was compiled with:

```bash
ee-gcc -O2 -G8 -mips2 -EL -fomit-frame-pointer \
       -fno-exceptions -fno-rtti -fno-merge-constants \
       -falign-functions=8 -g
```

## References

- Standard PS2 game decompilation structure (splitting .rodata, .text, etc.)
- Similar projects: Ocarina of Time, Majora's Mask, Super Mario 64
- PS2 SDK 2.3.0 documentation
- MIPS R5900 (Emotion Engine) architecture documentation

