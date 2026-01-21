# /media

The `/media` directory contains **non-compiled, late-bound system logic** used to define behavior, policy, and interaction semantics that do not require direct hardware execution.

This folder is intentionally **excluded from the compilation pipeline**.

---

## Purpose

`/media` exists to host logic that is:

- Architecture-agnostic
- Platform-aware but not platform-bound
- Replaceable at runtime
- Inspectable in plain text form
- Safe to omit, reload, or modify without rebuilding the system

It is not part of the execution core.

---

## Compilation Status

**Nothing in `/media` is compiled.**

- No object files
- No linking
- No ABI assumptions
- No toolchain dependency

All content is consumed via:
- Interpreters
- Loaders
- Dispatchers
- Message-passing interfaces
- Policy evaluators

If a component requires compilation, it does not belong here.

---

## What Belongs Here

The directory contains logic related to:

- Input semantics and control mapping
- Text selection and clipboard behavior
- Policy enforcement (auth, checks, cleaning)
- External dispatch coordination
- Fetching and caching behavior
- Temporary state handling
- Motion interpretation (not sensor drivers)
- Non-critical debug or auxiliary features
- Optional or conditional features

These components define **what should happen**, not **how hardware executes it**.

---

## What Must NOT Be Here

The following are explicitly forbidden in `/media`:

- Direct hardware access
- Memory-mapped I/O
- Interrupt handling
- Syscall implementation
- Scheduler interaction
- Pointer arithmetic assumptions
- Timing-critical loops
- Architecture-specific logic

Violating these rules breaks portability and defeats the purpose of this layer.

---

## Runtime Expectations

All `/media` components must assume:

- They may be loaded after boot
- They may be reloaded or discarded
- They may execute on different device classes
- They may run under different execution models
- They may be unavailable without causing system failure

Graceful degradation is mandatory.

---

## Platform Awareness

Platform differences (desktop vs Android-class devices) are handled by:

- Conditional bindings
- Capability checks
- Abstract input and gesture models
- Indirection via system services

No platform-specific code paths are hardwired.

---

## Stability Guarantees

- Changes to `/media` do not require rebuilding the OS
- Removal of `/media` does not prevent boot
- Errors in `/media` must not crash the system
- Core system correctness does not depend on `/media`

---

## Design Rationale

This directory enforces a strict separation between:

- **Mechanism** (compiled, minimal, hardware-bound)
- **Policy and behavior** (interpreted, replaceable, human-readable)

That separation is intentional and permanent.

---

## Position in PalisadeOS

The `/media` directory may be placed in one of two valid locations within the PalisadeOS source tree.

- `/PalisadeOS/media`  
  Use this location if `/media` is treated as a standalone or easily discoverable module at the project root.

- `/PalisadeOS/palisade/os/media`  
  Use this location if `/media` is intended to appear tightly integrated with the OS source layout.

Both placements are supported and equivalent in behavior.  
The choice affects **source organization only** and has no impact on runtime behavior, loading, or execution.

---

## Summary

`/media` is a **behavioral layer**, not an execution layer.

If a file needs to be compiled, optimized, or tightly bound to hardware, it does not belong here.
