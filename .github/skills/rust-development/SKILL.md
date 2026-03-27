---
name: rust-development
description: 'Use when implementing, refactoring, or extending Rust code. Covers modules, traits, structs, enums, ownership, error handling, Cargo changes, tests, and embedded no_std patterns.'
argument-hint: 'Describe the Rust feature, refactor, or API change to make'
---

# Rust Development

## When To Use
- Implement a new Rust feature or module
- Refactor existing Rust code without changing behavior
- Add structs, enums, traits, generics, or lifetimes
- Improve error handling, tests, or Cargo configuration
- Build or refactor embedded Rust code for no_std or bare-metal targets

## Procedure
1. Inspect the existing crate layout, public API surface, and dependency set before editing.
2. Prefer the simplest design that fits the current codebase. Do not introduce macros, complex trait hierarchies, or new crates without a concrete need.
3. For embedded targets, confirm runtime constraints early: `#![no_std]` and `#![no_main]` requirements, panic strategy, memory limits, and interrupt model.
4. Model ownership explicitly. Choose borrowed inputs when practical and owned outputs when data must outlive the caller.
5. Keep public APIs ergonomic. Prefer concrete types first, generics second, and `dyn Trait` only when callers need runtime polymorphism. Avoid exposing smart pointers and wrapper types in public APIs unless they are core to the design.
6. Keep types and functions focused. Prefer private fields by default, short parameter lists, and dedicated config or builder types when construction becomes complex.
7. Keep error handling explicit with `Result`, `Option`, or contextual error types. Avoid panics in normal control flow, and prefer dedicated error types over a single catch-all enum when failure modes differ materially.
8. Design for testability. Keep I/O and system interaction mockable, and feature-gate test-only escape hatches when they would weaken production guarantees.
9. Add or update tests for changed behavior. Keep unit tests near the module when they validate implementation details.
10. Validate with `cargo fmt`, `cargo clippy`, and `cargo test` when the workspace supports them. Use stronger checks such as `cargo audit`, `cargo udeps`, `cargo hack`, or `miri` when the change warrants them.

## Design Heuristics
- Use meaningful, descriptive names and standard Rust naming conventions.
- Prefer plain data structures before clever abstractions.
- Use traits for stable behavioral boundaries, not just to avoid duplication.
- Use `String` and `Vec<T>` only when ownership is required; otherwise consider `&str`, slices, and iterators.
- Prefer borrowing over ownership when it keeps APIs clear and avoids unnecessary allocation.
- Prefer `impl AsRef<Path>`, `impl AsRef<str>`, and similar bounds for function inputs when they improve ergonomics without infecting type definitions.
- Prefer `impl Read` or `impl Write` style one-shot I/O boundaries to keep parsing and transformation code reusable across files, buffers, and tests.
- Keep generic bounds readable. If a signature becomes difficult to parse, simplify it.
- Prefer standard library types in public APIs over third-party types unless the external type materially improves interoperability.
- For no_std code, prefer fixed-size arrays or `heapless` containers over heap-backed collections.
- Keep peripheral ownership explicit and single-source to prevent conflicting hardware access.
- Keep interrupt-shared state behind critical sections and minimal mutable scope.
- Avoid wildcard imports except where Rust convention makes them appropriate, such as `use super::*` in tests.
- Avoid redundant comments. Add comments for invariants, safety, and Rust-specific nuance instead of narrating obvious code.
- Add module docs and canonical rustdoc sections for public APIs when the behavior, errors, panics, or safety requirements are not self-evident.

## Delivery Checklist
- The API shape matches existing project patterns
- New dependencies are justified
- Error paths are handled intentionally
- Lint suppressions, unsafe blocks, and magic values are documented with reasons when present
- No stray `dbg!`, temporary `println!`, or commented-out code remains in finished changes
- Tests cover the changed behavior
- Formatting, linting, and tests were attempted when available