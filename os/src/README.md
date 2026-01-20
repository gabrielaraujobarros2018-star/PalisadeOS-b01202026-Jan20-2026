# PalisadeOS — Build b01202026

PalisadeOS b01202026 is a low-level, real-hardware–focused operating system build designed to boot across **Android devices**, **UEFI systems**, and **legacy BIOS PCs** using a single, disciplined boot philosophy. This build formalizes boot contracts, runtime entry semantics, and cross-architecture guarantees.

---

## Build Focus

This build is not feature-driven. It is **infrastructure-driven**.

Primary goals:

- Deterministic boot across firmware types
- Strict separation between firmware, bootloader, kernel, and programs
- Identical kernel semantics on x86_64 and ARM64
- Safe-by-default behavior on unknown hardware
- Real binaries that link, load, and execute on physical devices

---

## Supported Boot Environments

### Android Devices (ARM64)
- Android `boot.img` v3/v4 parsing
- Vendor DTB/DTBO acceptance (read-only)
- A/B slot and dynamic partition detection
- No reliance on Android userspace
- Fastboot-compatible recovery paths
- Hard failure on incompatible DT or memory maps

### UEFI Systems (x86_64 + ARM64)
- Full UEFI Boot Services consumption
- Clean `ExitBootServices`
- EFI memory map validation
- FAT32 ESP loading
- GRUB and direct EFI boot support
- EFI recovery image support

### Legacy BIOS (x86_64)
- MBR-safe boot path
- 16-bit → 32-bit → 64-bit transitions
- A20 handling
- E820 memory map parsing
- Serial-only debug as a requirement

---

## Program Runtime Model

This build introduces a **real program runtime ABI**:

- `_start` is the mandatory entry point
- `program_main` is the standardized program symbol
- Kernel services are accessed via explicit symbol shims
- No libc, no dynamic loader, no host assumptions

All programs are:
- Freestanding
- Statically linked
- Deterministic
- Inspectable

---

## Frameworks

In PalisadeOS, **Frameworks are system modules**, not application APIs.

They exist to:
- Centralize hardware and subsystem logic
- Enforce capability boundaries
- Avoid code duplication inside programs

Examples:
- Bluetooth acquisition frameworks
- Wi-Fi handling frameworks
- Hardware-facing service modules

---

## Safety & Hard-Fail Rules

This build enforces non-negotiable safety rules:

- Ambiguous firmware data → boot abort
- Inconsistent memory maps → boot abort
- Unknown storage layouts → read-only
- Missing recovery path → refuse installation

Silent failure is considered a bug.

---

## What This Build Is Not

- Not a toy OS
- Not an emulator-only project
- Not Android-based
- Not Linux-based
- Not dependent on vendor blobs at runtime

---

## Status

- Real objects compile and link
- Runtime entry is validated
- Cross-arch boot semantics are defined
- Firmware ownership is intentionally avoided

This build establishes the **foundation**. Everything else builds on top of it.

---

**Build:** `b01202026`  
**Philosophy:** correctness first, hardware always
