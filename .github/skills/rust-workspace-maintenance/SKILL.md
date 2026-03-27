---
name: rust-workspace-maintenance
description: 'Use when maintaining a Rust workspace locally: Cargo.toml changes, feature flags, dependency hygiene, linting, formatting, workspace layout, release preparation, build/test command hygiene, and embedded target configuration.'
argument-hint: 'Describe the Cargo, workspace, dependency, or maintenance task'
---

# Rust Workspace Maintenance

## When To Use
- Add or adjust workspace members, package metadata, or Cargo configuration
- Review dependency hygiene, feature flags, or version alignment
- Improve formatting, linting, and verification workflows
- Prepare a local crate or workspace for release, CI, or broader team use
- Investigate build friction caused by workspace layout or Cargo configuration
- Maintain embedded target setup, runner configuration, and no_std compatibility across crates

## Procedure
1. Inspect the workspace structure, relevant `Cargo.toml` files, and existing verification commands before making changes.
2. Keep dependency changes minimal. Prefer existing crates and standard library functionality when they are sufficient.
3. For embedded workspaces, verify target triples, `.cargo/config` runner settings, panic strategy, and linker/runtime assumptions are explicit.
4. Treat feature flags as additive and easy to reason about. Avoid features that silently remove functionality or break downstream combinations.
5. Keep workspace organization explicit. Split crates when responsibilities diverge enough to improve maintenance, testability, or compile times.
6. Keep crate boundaries and public APIs clean: prefer private fields by default, avoid leaking third-party types when practical, and keep imports disciplined.
7. Validate changes with the narrowest useful command first, then broaden to workspace-level checks.
8. Use stronger maintenance checks when the workspace warrants them, including `cargo audit`, `cargo udeps`, `cargo hack`, and `miri`.
9. Keep developer workflows predictable: formatting, clippy, tests, docs, and feature checks should be easy to discover and run locally.

## Maintenance Heuristics
- Prefer standard library types in public APIs to reduce dependency leakage across crates.
- Align versions and features deliberately rather than allowing accidental drift between workspace members.
- Prefer explicit crate responsibilities over catch-all utility crates.
- Keep release and validation steps documented close to the codebase when they are non-obvious.
- Review whether public types implement `Debug`, and `Display` when meant for humans.
- Avoid wildcard imports in production modules except for established prelude patterns.
- Keep the workspace free of commented-out code, stray `dbg!`, and temporary debug printing.
- In no_std crates, avoid accidental `std` leakage by reviewing default features and target-specific dependencies.
- Prefer deterministic memory strategies for embedded crates and document any optional allocator usage explicitly.

## Common Checks
- `cargo fmt --all`
- `cargo clippy --workspace --all-targets`
- `cargo test --workspace`
- `cargo check --workspace --target <target-triple>` when embedded targets are in scope
- `cargo doc --no-deps` when public API or docs changed materially
- `cargo hack check --feature-powerset` when feature combinations matter
- `cargo udeps --workspace` when dependency churn is being cleaned up

## Delivery Checklist
- Cargo changes are minimal and intentional
- Feature behavior remains additive and understandable
- Dependency additions are justified
- Validation commands appropriate to the workspace were attempted
- Workspace hygiene issues such as stray debug code and broad imports were checked