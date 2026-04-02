# Infrastructure Tracking

Track build system, tooling, dependencies, and infrastructure setup.

## Project Structure

### Workspace Layout

```
rust-ai-baseline/
├── Cargo.toml          # [ ] Workspace root
├── crates/
│   ├── agc-core/       # [ ] Core types (Scaled, Vector3, etc.)
│   ├── agc-memory/     # [ ] Memory model (erasable, fixed, channels)
│   ├── agc-executive/  # [ ] Task scheduling and waitlist
│   ├── agc-guidance/   # [ ] Guidance algorithms (P63, P64, P66, P67)
│   ├── agc-navigation/ # [ ] Navigation and IMU
│   ├── agc-io/         # [ ] I/O channels and DSKY
│   └── agc-sim/        # [ ] Complete simulation binary
├── specs/              # [x] Spec templates
├── transformation/     # [x] Progress tracking
└── tests/              # [ ] Integration tests
    └── historical/     # [ ] Apollo mission validation tests
```

**Status**: Planning - workspace structure not yet created

## Dependencies

### Core Dependencies

| Crate | Version | Purpose | Status |
|-------|---------|---------|--------|
| `thiserror` | latest | Error types | Not added |
| `bitflags` | latest | Bit manipulation | Not added |
| `serde` | latest | Serialization (optional) | Not added |

### Math and Numerics

| Crate | Version | Purpose | Status |
|-------|---------|---------|--------|
| `nalgebra` | latest | Vectors, matrices, quaternions | Not added |
| `fixed` | latest | Fixed-point arithmetic | Not added |
| TBD | | Alternative fixed-point library | Research needed |

### Testing and Validation

| Crate | Version | Purpose | Status |
|-------|---------|---------|--------|
| `proptest` | latest | Property-based testing | Not added |
| `criterion` | latest | Benchmarking | Not added |
| `approx` | latest | Floating-point comparison | Not added |

### Embedded/No-std (if applicable)

| Crate | Version | Purpose | Status |
|-------|---------|---------|--------|
| `heapless` | latest | No-heap collections | Not added |
| `embedded-hal` | latest | Hardware abstraction | Research needed |

### Dev Tools

| Crate | Version | Purpose | Status |
|-------|---------|---------|--------|
| `insta` | latest | Snapshot testing | Not added |

## Build Configuration

### Feature Flags

- [ ] `std` (default): Standard library support
- [ ] `no_std`: No standard library (embedded targets)
- [ ] `f64-math`: Use f64 for intermediate calculations
- [ ] `f32-math`: Use f32 for intermediate calculations
- [ ] `historical-validation`: Include Apollo mission test data
- [ ] `simulated-hardware`: Mock I/O for testing
- [ ] `benches`: Enable benchmark harness

### Targets

| Target | Purpose | Status |
|--------|---------|--------|
| `x86_64-unknown-linux-gnu` | Development/testing | Default |
| `x86_64-pc-windows-msvc` | Development/testing | Default |
| `wasm32-unknown-unknown` | Web demos | Research needed |
| `thumbv7em-none-eabihf` | Embedded demo (STM32) | Research needed |

## Tooling

### Development Tools

- [x] `cargo-watch` - Auto-recompilation
- [ ] `cargo-nextest` - Faster test runner
- [ ] `cargo-expand` - Macro expansion
- [ ] `cargo-audit` - Security audits
- [ ] `cargo-outdated` - Dependency updates
- [ ] `cargo-tree` - Dependency visualization
- [ ] `cargo-udeps` - Unused dependency detection
- [ ] `cargo-make` - Task automation

### Code Quality

- [ ] `rustfmt` - Code formatting (built-in)
- [ ] `clippy` - Linting (built-in)
- [ ] `cargo-deny` - Dependency policy enforcement
- [ ] `cargo-geiger` - Unsafe code detection

### Documentation

- [ ] `cargo doc` - Generate documentation
- [ ] `mdbook` - For guides/tutorials (optional)

## CI/CD

### GitHub Actions Workflows

- [ ] **Test**: Run `cargo test` on push
- [ ] **Clippy**: Run `cargo clippy -- -D warnings`
- [ ] **Format**: Check `cargo fmt -- --check`
- [ ] **Security**: Run `cargo audit`
- [ ] **Benchmarks**: Run criterion benchmarks on PR
- [ ] **Coverage**: Generate code coverage reports
- [ ] **Release**: Automated releases on tag push

### Status Badges

- [ ] Build status
- [ ] Test coverage
- [ ] Security audit
- [ ] Documentation status

## Performance Infrastructure

### Benchmarking

- [ ] Criterion setup for micro-benchmarks
- [ ] Full mission simulation benchmark
- [ ] Memory usage profiling
- [ ] Comparison with AGC timing (where applicable)

**Benchmark Targets**:
- Fixed-point arithmetic operations
- Vector/matrix operations
- Guidance algorithm update cycle
- Complete descent trajectory simulation

### Profiling

- [ ] Flamegraph generation setup
- [ ] Valgrind/cachegrind integration (Linux)
- [ ] Perf integration (Linux)

## Documentation Infrastructure

### API Documentation

- [ ] Rustdoc comments on all public APIs
- [ ] Module-level documentation
- [ ] Examples in documentation
- [ ] Cross-references to AGC source

### Guides

- [ ] README.md with quick start
- [ ] CONTRIBUTING.md
- [ ] Architecture guide
- [ ] Transformation methodology guide

## Development Environment

### VS Code Setup

- [x] rust-analyzer extension
- [x] Settings: formatOnSave, clippy on save
- [x] Tasks: cargo check, cargo test, cargo watch
- [x] Launch configs for debugging

### Recommended Extensions

- [x] rust-analyzer
- [ ] Error Lens
- [ ] CodeLLDB
- [ ] Even Better TOML
- [ ] GitLens

## Historical Validation Data

### Apollo Mission Data Sources

- [ ] Collect Apollo 11 descent telemetry
- [ ] Collect Apollo 12 precision landing data
- [ ] Collect Apollo 13 abort data (if relevant)
- [ ] Identify data format and parsing strategy

**Data Storage Strategy**:
- [ ] Store in `tests/data/` directory
- [ ] Format: TBD (JSON, CSV, binary?)
- [ ] Include metadata (mission, event, timestamp)
- [ ] Feature-gate large datasets

## Open Questions

1. **Fixed-point library**: Use `fixed` crate, `nalgebra`, or custom implementation?
2. **WASM target**: Worth supporting for web demos?
3. **Embedded target**: Choose specific embedded platform for demo?
4. **Historical data**: Best format for storing mission telemetry?
5. **Workspace vs single crate**: Start with workspace or single crate?

## Next Steps

1. [ ] Create initial Cargo.toml for workspace
2. [ ] Set up basic crate structure (agc-core as starting point)
3. [ ] Add essential dependencies (thiserror, proptest)
4. [ ] Create CI/CD workflow file
5. [ ] Set up first benchmark harness

## Notes

- Keep dependencies minimal initially
- Prefer standard library types in public APIs
- Feature-gate heavy dependencies (criterion, large test data)
- Document why each dependency is needed
