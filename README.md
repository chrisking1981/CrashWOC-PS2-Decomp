# Crash Bandicoot: The Wrath of Cortex - PS2 Decompilation

An open-source decompilation project for **Crash Bandicoot: The Wrath of Cortex** on the PlayStation 2.

## ⚠️ Legal Notice

**You must own an original copy of the game to use this project.**

This project contains only:
- Reverse-engineered source code
- Symbol tables and function names
- Research documentation
- Analysis tools

**This repository does NOT contain:**
- Game binaries (SLES_503.86 or similar)
- ISO files, ROMs, or copyrighted assets
- Sony SDK or proprietary binaries
- Build artifacts or copyrighted material

All code in this repository is original work created through reverse engineering. The project is for educational and preservation purposes.

## 📋 Project Status

**Early Stage** - Symbol extraction and analysis phase

- ✅ Symbol extraction complete (3,751 functions, 3,368 variables)
- ✅ Compiler identification (GCC 2.95.2, SDK 2.3.0)
- ✅ Debug information analysis
- 🔄 Source code reconstruction (in progress)
- ⏳ Bit-matched compilation (pending)

## 🛠️ Toolchain Setup

### Prerequisites

- **WSL (Windows Subsystem for Linux)** or native Linux environment
- **Ghidra** with R5900 processor module
- **Sony PlayStation 2 SDK 2.3.0** (for bit-matching)
  - Contains ee-gcc 2.95.2
  - Required for exact compiler matching

### Installing the PS2 Toolchain

#### Option 1: Modern ps2dev Toolchain (WSL/Linux)

```bash
# Clone PS2 toolchain repository
git clone https://github.com/ps2dev/ps2toolchain.git
cd ps2toolchain

# Build the toolchain
chmod +x toolchain.sh
./toolchain.sh

# Set environment variables (~/.bashrc)
export PS2DEV="/usr/local/ps2dev"
export PS2SDK="$PS2DEV/ps2sdk"
export PATH="$PATH:$PS2DEV/bin:$PS2DEV/ee/bin:$PS2DEV/iop/bin:$PS2DEV/dvp/bin:$PS2SDK/bin"

# Verify installation
ee-gcc --version
```

**Note:** Modern ps2dev uses GCC 3.2.3+, not 2.95.2. For bit-matching, you'll need the original Sony SDK 2.3.0 with ee-gcc 2.95.2.

#### Option 2: Original Sony SDK 2.3.0

The original game was compiled with:
- **Compiler**: ee-gcc 2.95.2
- **SDK**: Sony PlayStation 2 SDK 2.3.0
- **C Library**: Newlib (embedded in SDK)

For bit-matched decompilation, you need the exact same toolchain. The original SDK is not publicly available, but you may be able to find it through legal channels if you're a licensed developer.

### Compiler Flags

Based on reverse engineering analysis, the original was compiled with:

```bash
ee-gcc \
    -O2 \
    -G8 \
    -mips2 \
    -EL \
    -fomit-frame-pointer \
    -fno-exceptions \
    -fno-rtti \
    -fno-merge-constants \
    -falign-functions=8 \
    -g \
    -I/usr/local/sce/ee/include \
    -c source.c \
    -o source.o
```

## 📁 Repository Structure

```
CrashWOC-PS2-Decomp/
├── src/                        # C source reconstruction (empty for now)
├── symbols/                    # Function & variable names
│   ├── FUNCTIONS.txt           # All function addresses and names
│   ├── VARIABLES.txt            # All variable addresses and names
│   └── ALL_SYMBOLS.txt         # Complete symbol list
├── docs/                       # Research notes and documentation
│   ├── ADVANCED_DECOMPILE_ANALYSIS.txt
│   └── project_structure.md
├── tools/                      # Scripts for Ghidra/analysis
└── ghidra/                     # Exported symbols & datatypes from Ghidra
```

See [docs/project_structure.md](docs/project_structure.md) for detailed structure information.

## 🔧 Importing Symbols into Ghidra

### Method 1: Using the Import Script

1. Open your Ghidra project with the SLES_503.86 binary loaded
2. Run the import script:

```bash
python tools/ghidra_import.py
```

### Method 2: Manual Import

1. In Ghidra, go to **File → Import File**
2. Select `ghidra/symbols.xml` (if available)
3. Or manually import symbols from `symbols/FUNCTIONS.txt` and `symbols/VARIABLES.txt`

### Method 3: Using Ghidra Scripts

Ghidra scripts can be created to bulk-import symbols from the text files. See `tools/` directory for example scripts.

## 🗺️ Roadmap to Bit-Matching

The goal is to achieve **bit-matched decompilation** - where the recompiled binary matches the original byte-for-byte.

### Phase 1: Symbol Extraction ✅
- Extract all function names and addresses
- Extract all variable names and addresses
- Identify compiler and SDK version

### Phase 2: Toolchain Setup 🔄
- Obtain or build ee-gcc 2.95.2
- Configure compiler flags
- Set up build system

### Phase 3: Source Reconstruction
- Start with known code (Newlib functions)
- Reconstruct simple engine functions
- Work up to complex game systems

### Phase 4: Bit-Matching Verification
- Compare compiled output with original
- Adjust compiler flags as needed
- Verify function-by-function matching

### Phase 5: Documentation & Cleanup
- Document all functions
- Clean up code structure
- Create comprehensive documentation

## 📊 Project Statistics

- **Total Functions**: 3,751 (with debug info)
- **Total Variables**: 3,368 (with debug info)
- **Source Files Identified**: 267
- **Symbols Total**: 11,227
- **Debug Format**: MDEBUG/STABS (759 KB of debug data!)

## 🔍 Key Findings

- **Compiler**: GCC 2.95.2 (ee-gcc from Sony PS2 SDK 2.3.0)
- **SDK Version**: Sony PlayStation 2 SDK 2.3.0
- **Engine**: Nu Engine 2 (Traveller's Tales proprietary engine)
- **Developer**: Traveller's Tales (now TT Games)
- **Build Type**: Debug build with extensive symbol information

See [docs/ADVANCED_DECOMPILE_ANALYSIS.txt](docs/ADVANCED_DECOMPILE_ANALYSIS.txt) for detailed analysis.

## 🤝 Contributing

Contributions are welcome! This is a community effort to preserve and understand this classic game.

### How to Contribute

1. Fork the repository
2. Create a branch for your work
3. Make your changes
4. Submit a pull request

### Areas Needing Help

- Source code reconstruction
- Function documentation
- Build system improvements
- Testing and verification

## 📝 Credits

- **Original Game**: Developed by Traveller's Tales, published by Universal Interactive
- **Decompilation Project**: Community effort
- **Tools Used**: Ghidra, IDA Pro, ps2dev toolchain

## 📚 Resources

- [PS2 Development Documentation](https://lukasz.dk/playstation-2-programming/)
- [ps2dev Toolchain](https://github.com/ps2dev/ps2toolchain)
- [PS2 SDK](https://github.com/ps2dev/ps2sdk)
- [Ghidra](https://ghidra-sre.org/)

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

**Note**: This is a work in progress. The codebase is currently empty and will be populated as reverse engineering progresses.

