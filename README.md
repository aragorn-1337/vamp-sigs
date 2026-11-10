# VAMP Signature Database

Pre-generated VAMP (Vivisect Analysis Module for Pattern Matching) signatures
for identifying known library functions in stripped binaries.

## Overview

This database contains JSON-formatted function signatures generated from `.a`
static archives using instruction-aware masking. Signatures can be loaded by
Vivisect's VAMP analysis module to automatically identify and name library
functions in stripped or statically-linked binaries.

## Signature Coverage

| Library | Versions | Architectures |
|---------|----------|---------------|
| glibc | 2.27, 2.31, 2.35, 2.39, 2.43 | amd64, i386, arm, aarch64 |
| musl | 1.2.4 | amd64, i386, arm, aarch64 |
| OpenSSL | 1.0.2g, 1.1.1f, 3.0.2, 3.0.13, 3.4.1, 3.5.5 | amd64, aarch64 |
| libstdc++ | 13.3.0, 14.2.0 | amd64 |
| libgcc | 13.3.0 | amd64 |
| zlib | 1.3 | amd64 |
| libbz2 | 1.0.8 | amd64 |
| liblzma | 5.4.5 | amd64 |
| liblz4 | 1.9.4 | amd64 |
| zstd | 1.5.5 | amd64 |
| snappy | 1.1.10 | amd64 |
| libcurl | 8.5.0 | amd64 |
| sqlite | 3.45.1 | amd64 |
| libuv | 1.48.0 | amd64 |
| libexpat | 2.6.1 | amd64 |
| libffi | 3.4.6 | amd64 |
| libprotobuf | 3.21.12 | amd64 |
| json-c | 0.17 | amd64 |

**Stats:** 57 sig sets, 195,366 signatures, 91.3% with instruction-aware masking.

## Installation

### Option 1: Clone into the default Vivisect data directory

```bash
mkdir -p ~/.viv/vamp-sigs
git clone https://github.com/atlas0fd00m/vamp-sigs.git ~/.viv/vamp-sigs
```

### Option 2: Set VAMP_SIG_DIR environment variable

```bash
git clone https://github.com/atlas0fd00m/vamp-sigs.git /path/to/sigs
export VAMP_SIG_DIR=/path/to/sigs/data
```

### Option 3: Use with Vivisect

The Vivisect VAMP analysis module searches for sigs in this order:
1. `VAMP_SIG_DIR` environment variable
2. `~/.viv/vamp-sigs/data/` (default user directory)
3. `vivisect/vamp/data/` (package-level, for bundled MSVC sigs only)

## Directory Structure

```
vamp-sigs/
├── data/                  # JSON signature files
│   ├── index.json         # Master index
│   ├── glibc_2.39_x64.json
│   ├── musl_1.2.4_x64.json
│   ├── openssl_3.0.13_x64.json
│   └── ...
├── tools/                 # Signature generation and validation tools
│   ├── gen_elf_sigs.py    # Generate sigs from .a/.so ELF libraries
│   ├── gen_sigs.py        # Full vivisect-based sig generator (PE/ELF)
│   ├── validate_sigs.py   # TP/FP/FN validation metrics
│   └── sig_diff.py        # Version diff tool
├── LICENSE
└── README.md
```

## Generating New Signatures

```bash
# From a .a static archive (preferred for static binary matching)
python3 tools/gen_elf_sigs.py \
    --input /usr/lib/x86_64-linux-gnu/libc.a \
    --library glibc \
    --version 2.39 \
    --arch amd64 \
    --platform linux \
    --compiler gcc-13 \
    --output data/glibc_2.39_x64.json

# From a .so shared library (fallback)
python3 tools/gen_elf_sigs.py \
    --input /lib/x86_64-linux-gnu/libc.so.6 \
    --library glibc \
    --version 2.39 \
    --arch amd64 \
    --output data/glibc_2.39_x64.json
```

## License

Same license as the Vivisect project.