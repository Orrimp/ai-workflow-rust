# Infrastructure

## Project Structure (MVP)

```
agc-executive/          # single crate for Milestone 1
├── Cargo.toml
├── src/
│   ├── lib.rs
│   ├── executive.rs    # job table, priority scheduler
│   ├── waitlist.rs     # delta-time deferred task queue
│   └── types.rs        # fixed-point types, AGC word primitives
├── tests/
│   └── scheduler.rs    # integration-level scheduler tests
└── specs/              # symlink or copy of workspace specs/
```

Split into a workspace (`crates/`) only once Milestone 2 (navigation component) starts.

## Dependencies (MVP)

| Crate | Purpose |
|-------|---------|
| `heapless` | `no_std` fixed-capacity collections (job queue, WAITLIST ring) |
| `fixed` | Fixed-point arithmetic (decide via ADR-003 spike) |
| `bitflags` | AGC I/O channel / status word modelling |
| `proptest` | Property-based scheduler tests |
| `cargo-nextest` | Fast test runner |
| `cargo-audit` | Security advisory check |

## Targets

| Target | Purpose |
|--------|---------|
| `x86_64-unknown-linux-gnu` | Development/CI |
| `xtensa-esp8266-none-elf` | Long-term embedded target (no_std) |

## Feature Flags

- `std` (default) — standard library + `std` collections
- `no_std` — bare-metal; enables `heapless` paths

## CI Checks (planned)

```sh
cargo fmt -- --check
cargo clippy -- -D warnings
cargo nextest run
cargo audit
```
