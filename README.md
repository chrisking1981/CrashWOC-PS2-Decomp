# Crash Bandicoot: The Wrath of Cortex - PS2 Decompilation

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Status: Early Development](https://img.shields.io/badge/Status-Early%20Development-orange)](https://github.com)

An open-source decompilation project for **Crash Bandicoot: The Wrath of Cortex** on the PlayStation 2. This project aims to reverse engineer the game's source code to achieve bit-matched decompilation, enabling better understanding, preservation, and potential improvements to this classic game.

## 🎯 Project Goals

- **Bit-matched decompilation**: Recompile the source code to produce a byte-for-byte identical binary
- **Code documentation**: Understand and document the game's internal systems
- **Preservation**: Ensure the game's technical details are preserved for future generations
- **Education**: Learn about PS2 game development, reverse engineering, and decompilation techniques

## ⚠️ Legal Notice

**You must own an original copy of the game to use this project.**

This repository contains only:
- Reverse-engineered source code (once reconstructed)
- Symbol tables and function names extracted from debug information
- Research documentation and analysis
- Analysis tools and scripts

**This repository does NOT contain:**
- Game binaries (SLES_503.86 or similar)
- ISO files, ROMs, or copyrighted assets
- Sony SDK or proprietary binaries
- Build artifacts or copyrighted material

All code in this repository is original work created through reverse engineering. The project is for educational and preservation purposes only.

## 📊 Project Status

**Current Phase: Symbol Extraction & Analysis** ✅

### Completed ✅
- ✅ **Symbol Extraction**: All 3,751 functions and 3,368 variables extracted from debug information
- ✅ **Compiler Identification**: Confirmed GCC 2.95.2 (ee-gcc) from Sony PS2 SDK 2.3.0
- ✅ **SDK Analysis**: Identified Sony PlayStation 2 SDK 2.3.0 with extensive debug information
- ✅ **Debug Information Analysis**: Parsed 759 KB of MDEBUG/STABS debug data
- ✅ **Source File Identification**: 267 source files identified from debug info
- ✅ **Repository Structure**: Clean, open-source ready structure established

### In Progress 🔄
- 🔄 **Source Code Reconstruction**: Beginning with simple functions
- 🔄 **Toolchain Setup**: Configuring ee-gcc 2.95.2 for bit-matching

### Planned ⏳
- ⏳ **Bit-Matched Compilation**: Verify function-by-function matching
- ⏳ **Complete Source Reconstruction**: All 3,751 functions
- ⏳ **Documentation**: Comprehensive function documentation
- ⏳ **Build System**: Automated compilation and verification

## 🔍 How Symbols Were Extracted

The function names and variable names in this repository were extracted using **AI-assisted analysis** of the original PS2 ISO file. The extraction process involved:

1. **ISO Extraction**: The PS2 game ISO was extracted to obtain the ELF binary (SLES_503.86)
2. **Binary Analysis**: The binary was analyzed to locate debug information sections
3. **AI-Assisted Symbol Extraction**: AI tools were used to parse and extract symbols from the debug information embedded in the binary

The symbols come from the **debug information** embedded in the original PS2 binary (SLES_503.86), which was compiled as a debug build with the `-g` flag. This is extremely rare for commercial games, which are typically compiled in release mode without debug information.

### Debug Information Sources

The binary contains extensive debug information:

1. **MDEBUG Section** (MIPS-specific debug format)
   - Offset: `0x00534044`
   - Size: `778,136 bytes` (759 KB!)
   - Contains: Function names, variable names, source file paths, line numbers

2. **STABS Section** (GCC debug format)
   - Offset: `0x005F3C70`
   - Size: `111,996 bytes`
   - Contains: Additional symbol and type information

3. **Symbol Table (.symtab)**
   - Offset: `0x00621450`
   - Size: `179,632 bytes`
   - Contains: `11,227 total symbols` (3,751 functions + 3,368 variables)

4. **String Table (.strtab)**
   - Offset: `0x0064D200`
   - Size: `124,403 bytes`
   - Contains: Symbol name strings

### Extraction Methods

The symbol extraction was performed using AI-assisted tools that:
- Parsed the ELF binary structure
- Identified and extracted the MDEBUG and STABS debug sections
- Parsed the symbol table (.symtab) and string table (.strtab)
- Cross-referenced debug information to build complete symbol lists
- Generated the FUNCTIONS.txt, VARIABLES.txt, and ALL_SYMBOLS.txt files

The AI tools efficiently processed the 759 KB of debug data and extracted all 11,227 symbols that are now available in this repository.

## 🛠️ Setup & Installation

### Prerequisites

- **WSL (Windows Subsystem for Linux)** or native Linux environment
- **Ghidra** with R5900 processor module installed
- **Sony PlayStation 2 SDK 2.3.0** (for bit-matching) - *Not publicly available, requires legal acquisition*
  - Contains ee-gcc 2.95.2
  - Required for exact compiler matching

### Step 1: Clone the Repository

```bash
git clone https://github.com/yourusername/CrashWOC-PS2-Decomp.git
cd CrashWOC-PS2-Decomp
```

### Step 2: Install PS2 Toolchain

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

### Step 3: Setup Ghidra

1. Download and install [Ghidra](https://ghidra-sre.org/)
2. Install the R5900 processor module (included by default)
3. Import your SLES_503.86 binary into a Ghidra project
4. Import symbols using one of the methods below

### Step 4: Import Symbols into Ghidra

#### Method 1: Using the Import Script

1. Open your Ghidra project with the SLES_503.86 binary loaded
2. Run the import script:

```bash
python tools/ghidra_import.py
```

#### Method 2: Manual Import

1. In Ghidra, go to **File → Import File**
2. Select `ghidra/symbols.xml` (if available)
3. Or manually import symbols from `symbols/FUNCTIONS.txt` and `symbols/VARIABLES.txt`

#### Method 3: Using Ghidra Scripts

Ghidra scripts can be created to bulk-import symbols from the text files. See `tools/` directory for example scripts.

### Step 5: Verify Setup

```bash
# Check that all symbol files are present
ls -la symbols/

# Should show:
# FUNCTIONS.txt
# VARIABLES.txt
# ALL_SYMBOLS.txt
```

## 📁 Project Structure

```
CrashWOC-PS2-Decomp/
├── src/                        # C source reconstruction (empty for now)
│   ├── nu_engine/             # Nu Engine 2 functions
│   ├── game/                   # Game-specific code
│   └── lib/                    # Library wrappers
├── symbols/                    # Function & variable names
│   ├── FUNCTIONS.txt           # All function addresses and names (3,751 functions)
│   ├── VARIABLES.txt           # All variable addresses and names (3,368 variables)
│   └── ALL_SYMBOLS.txt         # Complete symbol list (11,227 symbols)
├── docs/                       # Research notes and documentation
│   ├── ADVANCED_DECOMPILE_ANALYSIS.txt  # Detailed compiler/SDK analysis
│   └── project_structure.md    # Intended folder layout documentation
├── tools/                      # Scripts for Ghidra/analysis
│   ├── ghidra_import.py        # Import symbols into Ghidra
│   └── bit_match_compare.py    # Compare compiled output with original
├── ghidra/                     # Exported symbols & datatypes from Ghidra
│   ├── symbols.xml             # Ghidra symbol export (if available)
│   └── datatypes.gdt           # Ghidra datatype export (if available)
├── .gitignore                  # Excludes binaries, Ghidra cache, etc.
├── LICENSE                     # MIT License
└── README.md                   # This file
```

See [docs/project_structure.md](docs/project_structure.md) for detailed structure information.

## 🗺️ Roadmap

### Phase 1: Symbol Extraction ✅
- [x] Extract all function names and addresses
- [x] Extract all variable names and addresses
- [x] Identify compiler and SDK version
- [x] Analyze debug information

### Phase 2: Toolchain Setup 🔄
- [ ] Obtain or build ee-gcc 2.95.2
- [ ] Configure compiler flags
- [ ] Set up build system
- [ ] Verify toolchain matches original

### Phase 3: Source Reconstruction
- [ ] Start with known code (Newlib functions)
- [ ] Reconstruct simple engine functions
- [ ] Work up to complex game systems
- [ ] Document all functions

### Phase 4: Bit-Matching Verification
- [ ] Compare compiled output with original
- [ ] Adjust compiler flags as needed
- [ ] Verify function-by-function matching
- [ ] Achieve bit-matched binary

### Phase 5: Documentation & Cleanup
- [ ] Document all functions
- [ ] Clean up code structure
- [ ] Create comprehensive documentation
- [ ] Prepare for public release

## 🤝 Contributing

Contributions are welcome! This is a community effort to preserve and understand this classic game.

### How to Contribute

1. **Fork the repository**
2. **Create a branch** for your work (`git checkout -b feature/your-feature-name`)
3. **Make your changes**
4. **Test your changes** (if applicable)
5. **Submit a pull request** with a clear description of your changes

### Contribution Guidelines

- **Code Style**: Follow existing code style and conventions
- **Documentation**: Document your functions and changes
- **Testing**: Test your changes before submitting
- **Commits**: Write clear, descriptive commit messages

### Areas Needing Help

- **Source Code Reconstruction**: Help reverse engineer functions
- **Function Documentation**: Document what functions do
- **Build System**: Improve build scripts and automation
- **Testing**: Test and verify reconstructed code
- **Research**: Analyze game systems and mechanics

### Getting Help

- **Issues**: Open an issue for bugs, questions, or feature requests
- **Discussions**: Use GitHub Discussions for general questions
- **Pull Requests**: Submit PRs for code contributions

## 📊 Project Statistics

- **Total Functions**: 3,751 (with debug info)
- **Total Variables**: 3,368 (with debug info)
- **Source Files Identified**: 267
- **Symbols Total**: 11,227
- **Debug Format**: MDEBUG/STABS (759 KB of debug data!)
- **Binary Size**: 6.73 MB
- **Code + Data Size**: 5.45 MB

## 🔍 Key Findings

- **Compiler**: GCC 2.95.2 (ee-gcc from Sony PS2 SDK 2.3.0)
- **SDK Version**: Sony PlayStation 2 SDK 2.3.0
- **Engine**: Nu Engine 2 (Traveller's Tales proprietary engine)
- **Developer**: Traveller's Tales (now TT Games)
- **Build Type**: Debug build with extensive symbol information
- **Entry Point**: 0x00100008
- **GP Register**: 0x00634970

See [docs/ADVANCED_DECOMPILE_ANALYSIS.txt](docs/ADVANCED_DECOMPILE_ANALYSIS.txt) for detailed analysis.

## 📚 Resources

### Documentation
- [PS2 Development Documentation](https://lukasz.dk/playstation-2-programming/)
- [PS2 SDK Documentation](https://github.com/ps2dev/ps2sdk)
- [Ghidra Documentation](https://ghidra-sre.org/)

### Tools
- [ps2dev Toolchain](https://github.com/ps2dev/ps2toolchain)
- [PS2 SDK](https://github.com/ps2dev/ps2sdk)
- [Ghidra](https://ghidra-sre.org/)

### Related Projects
- [Ocarina of Time Decompilation](https://github.com/zeldaret/oot)
- [Majora's Mask Decompilation](https://github.com/zeldaret/mm)
- [Super Mario 64 Decompilation](https://github.com/n64decomp/sm64)

## 📝 Credits

- **Original Game**: Developed by Traveller's Tales, published by Universal Interactive
- **Decompilation Project**: Community effort
- **Tools Used**: Ghidra, IDA Pro, ps2dev toolchain, AI-assisted analysis tools
- **Special Thanks**: All contributors and the reverse engineering community

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

**Note**: This is a work in progress. The codebase is currently empty and will be populated as reverse engineering progresses. All contributions are welcome!
