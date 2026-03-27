---
name: rust-debugging
description: 'Use when debugging Rust compiler errors, borrow checker issues, clippy warnings, failing tests, panics, async bugs, performance regressions, or embedded no_std failures.'
argument-hint: 'Describe the compiler error, failing test, panic, or runtime issue'
---

# Rust Debugging

## When To Use
- Investigate compiler or borrow checker errors
- Fix failing Rust tests or runtime panics
- Resolve clippy warnings that signal correctness or maintainability issues
- Diagnose async coordination problems or suspicious performance regressions
- Diagnose embedded no_std issues such as ISR races, hard faults, or peripheral ownership conflicts

## Procedure
1. Reproduce the issue with the smallest relevant command, usually a targeted `cargo test`, `cargo check`, or `cargo clippy` invocation.
2. Read the full error chain before editing. In Rust, the first type mismatch or move often causes several downstream errors.
3. For embedded targets, confirm whether the failure is target-specific and reproduce with the correct target triple and runner.
4. Separate the problem into one of these buckets: ownership and lifetime, type mismatch, trait resolution, async behavior, interrupt/concurrency behavior, or logic bug.
5. Fix the root cause instead of adding clones, boxing, `Arc<Mutex<_>>`, or public wrapper types as a reflex.
6. When the issue involves APIs or tests, check whether the design is hiding I/O, leaking dependency types, relying on panics for normal flow, or making mocking harder than necessary.
7. Re-run the narrow validation command, then broaden to repository-level checks if the fix touches shared code.

## Debugging Heuristics
- For borrow checker issues, identify who owns the value, who borrows it, and how long each reference must live.
- For trait errors, inspect the concrete type at the failure site and confirm the expected impl actually exists in scope.
- For panics, locate the invariant being assumed and decide whether the code should validate, propagate, or restructure instead.
- For async bugs, look for missed `.await` boundaries, lock scope problems, channel closure, and task cancellation.
- For performance regressions, identify the hot path before editing. Look first for unnecessary allocation, cloning, string building, repeated hashing, and avoidable copying.
- For long-running async tasks, check whether the code needs cooperative yield points to avoid starving the executor.
- For unsafe code, confirm the safety argument is explicit and validate with stronger tooling when practical.
- When writing fix code, use `snake_case` for names, `PascalCase` for types, and `SCREAMING_SNAKE_CASE` for constants. Avoid wildcard imports except `use super::*` inside test modules.
- For embedded panics and faults, verify panic handler choice, target memory layout, and startup/runtime configuration before changing application logic.
- For ISR issues, avoid blocking work in interrupt context and move heavy work to deferred paths when practical.
- For interrupt-shared state, confirm critical sections and ownership boundaries are explicit.

## Exit Criteria
- The failure reproduces before the fix and no longer reproduces after it
- The chosen fix preserves intended semantics
- New clones, allocations, or synchronization primitives are justified
- Any lint suppression or unsafe change has a clear reason
- Regression tests or assertions were added when practical
- No stray `dbg!`, temporary `println!`, or commented-out code remains in the applied fix