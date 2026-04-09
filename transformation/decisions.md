# Architecture Decision Records (ADRs)

## ADR-001: Spec-Driven Development

**Date**: 2026-04-02 | **Status**: Accepted

**Context**: Transforming AGC assembly to Rust requires capturing domain knowledge (scaling factors, timing, restart semantics) before writing code. Ad-hoc porting is fragile; specs enforce that design choices are explicit and reviewable.

**Decision**: Write a spec file for each AGC component before implementing it. Templates in `specs/` cover routines, modules, guidance algorithms, interrupts, and data structures.

**Rationale**: Design errors are cheaper to fix in a spec than in code; specs double as living documentation and test oracles.

---

## ADR-002: Workspace vs Single Crate (DRAFT)

**Status**: Proposed — decide before creating `Cargo.toml`

**Recommendation**: Start with a single crate (`agc-executive`). Split into a workspace only if Milestone 2 (navigation component) introduces a genuine need for separate compilation units or crate-level encapsulation.

---

## ADR-003: Fixed-Point Arithmetic Strategy (DRAFT)

**Status**: Proposed — spike needed before implementing core types

**Options**:
1. `fixed` crate — mature, `no_std`-compatible, type-safe scaling
2. Custom `Scaled<const B: i32>(i32)` newtype — full AGC-specific control, compile-time scale factor enforcement

**Next step**: Implement the same AGC expression (e.g., a velocity update at `B-7`) in both and pick the more readable/ergonomic option.

---

## ADR-004: Reimplementation (not emulation)

**Date**: 2026-04-02 | **Status**: Accepted

**Decision**: Port the EXECUTIVE scheduler semantics to idiomatic Rust. Do not build a cycle-accurate AGC binary emulator.

**Rationale**: The presentation deliverable is *methodology*, not a working AGC simulator. Idiomatic Rust is more readable for the audience and is the prerequisite for the long-term `no_std` embedded target.
