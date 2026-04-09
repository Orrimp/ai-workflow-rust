# Transformation Progress

**Last Updated**: [Date]

## Milestones

### Milestone 1: EXECUTIVE + WAITLIST (MVP)

- [ ] Crate scaffold (`agc-executive`, `no_std`-ready, `Cargo.toml`)
- [ ] Fixed-point type(s) for AGC scaled integers
- [ ] Static job table — 7 slots, priority ordering
- [ ] `STARTJOB` / `ENDJOB` / `NOVAC` semantics
- [ ] WAITLIST timer queue — up to 9 pending tasks, delta-time chain
- [ ] Phase-table restart protection
- [ ] All scheduler unit tests pass (`cargo nextest run`)
- [ ] `cargo clippy -- -D warnings` clean

**Status**: Not Started

### Milestone 2: One Navigation Component (optional)

Choose one: `SERVICER`, `ORBITAL_INTEGRATION`, or `CONIC_SUBROUTINES`

- [ ] Spec file written and reviewed
- [ ] Implementation scheduled via EXECUTIVE
- [ ] Output matches Virtual AGC emulator for same input state

**Status**: Not Started — depends on Milestone 1

## Active Work

_No active work items_

## Metrics

| | Count |
|---|---|
| Rust source files | 0 |
| Unit tests passing | 0 |
| Clippy warnings | — |
