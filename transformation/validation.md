# Validation and Testing Tracking

Track test coverage, validation status, and quality metrics.

## Test Coverage Overview

| Component | Unit Tests | Integration Tests | Property Tests | Historical Tests | Status |
|-----------|------------|-------------------|----------------|------------------|--------|
| Core Types | 0/0 | 0/0 | 0/0 | 0/0 | Not Started |
| Memory Model | 0/0 | 0/0 | 0/0 | 0/0 | Not Started |
| Executive | 0/0 | 0/0 | 0/0 | 0/0 | Not Started |
| Guidance | 0/0 | 0/0 | 0/0 | 0/0 | Not Started |
| Navigation | 0/0 | 0/0 | 0/0 | 0/0 | Not Started |
| I/O | 0/0 | 0/0 | 0/0 | 0/0 | Not Started |
| **TOTAL** | **0/0** | **0/0** | **0/0** | **0/0** | **0%** |

## Unit Tests

### Completed

_No unit tests yet_

### In Progress

_No tests in progress_

### Planned

- Fixed-point arithmetic tests (Scaled<N, T>)
- Vector3/Quaternion operation tests
- Memory address translation tests
- AGC word serialization/deserialization tests

## Integration Tests

### Completed

_No integration tests yet_

### In Progress

_No tests in progress_

### Planned

- Complete guidance cycle (initialization → update → termination)
- Task scheduling and waitlist interaction
- DSKY verb/noun interface
- Full descent profile (P63 → P64 → P66)

## Property-Based Tests

### Completed

_No property tests yet_

### Planned

- Fixed-point arithmetic roundtrip properties
- Scaling factor preservation
- Inverse operation properties (e.g., `from_agc_words(to_agc_words(x)) == x`)
- Quaternion normalization properties

## Historical Validation Tests

Target: Validate against real Apollo mission data

### Apollo 11

- [ ] **Descent Trajectory**: Validate P63/P64/P66 against known trajectory
  - Source: [Reference needed]
  - Data points: Altitude, velocity, attitude at key timestamps
  - Tolerance: TBD

- [ ] **State Vector**: Compare state vector at PDI (Powered Descent Initiation)
  - Source: [Reference needed]
  - Tolerance: TBD

### Apollo 12

- [ ] **Precision Landing**: Validate targeting accuracy
  - Source: [Reference needed]
  - Expected: Landing within meters of Surveyor 3
  - Tolerance: TBD

### Apollo 13

- [ ] **Abort Sequence**: Validate P67 abort logic (if implementing)
  - Source: [Reference needed]
  - Tolerance: TBD

### Apollo 14-17

- [ ] **Various Scenarios**: Additional validation as needed
  - Source: TBD

## Test Oracles

### AGC Simulator Comparison

- [ ] Set up Virtual AGC for reference comparisons
- [ ] Define interface for feeding identical inputs
- [ ] Compare outputs at key points
- [ ] Document acceptable deviations

### Known Values

Collect test cases with known correct values:

| Test Case | Input | Expected Output | Source | Status |
|-----------|-------|-----------------|--------|--------|
| _Examples TBD_ | | | | |

## Quality Metrics

### Code Coverage

- **Target**: 80% line coverage for core algorithms
- **Current**: N/A (no code yet)
- **Tool**: `cargo tarpaulin` or `cargo llvm-cov`

### Clippy

- **Policy**: Zero warnings with `-- -D warnings`
- **Current Warnings**: 0 (no code yet)

### Documentation Coverage

- **Target**: 100% of public APIs documented
- **Current**: N/A

## Test Infrastructure

### Test Data Management

```
tests/
├── data/
│   ├── apollo11/
│   │   ├── descent.json
│   │   └── state_vectors.json
│   ├── apollo12/
│   └── ...
├── fixtures/
│   ├── agc_words.rs
│   └── test_states.rs
└── integration/
    ├── guidance_tests.rs
    └── executive_tests.rs
```

**Status**: Not created

### Test Helpers

- [ ] Fixture builders for common test scenarios
- [ ] AGC word parsing utilities
- [ ] Tolerance comparison helpers for fixed-point values
- [ ] Mock I/O channels for testing

### Snapshot Testing

- [ ] Consider `insta` for regression testing
- [ ] Capture guidance outputs at each cycle
- [ ] Review and commit snapshots

## Benchmark Tracking

### Micro-benchmarks

| Benchmark | Target | Current | Status |
|-----------|--------|---------|--------|
| Fixed-point multiply | < 10ns | N/A | Not Started |
| Vector dot product | < 20ns | N/A | Not Started |
| Quaternion multiply | < 50ns | N/A | Not Started |
| Guidance update cycle | < 100µs | N/A | Not Started |

### Macro-benchmarks

| Benchmark | Target | Current | Status |
|-----------|--------|---------|--------|
| Full descent (P63→P66) | < 1s | N/A | Not Started |
| 10,000 guidance cycles | < 1s | N/A | Not Started |

### Memory Usage

| Component | Target | Current | Status |
|-----------|--------|---------|--------|
| Core types size | Minimal | N/A | Not Started |
| Heap allocations | Zero in critical path | N/A | Not Started |
| Stack usage per cycle | < 4KB | N/A | Not Started |

## Validation Milestones

### Milestone 1: Core Types Validated

- [ ] All fixed-point tests passing
- [ ] Vector/quaternion tests passing
- [ ] Property tests passing
- [ ] Benchmarks meet targets

### Milestone 2: Integration Tests Passing

- [ ] Task scheduling tests passing
- [ ] Guidance cycle tests passing
- [ ] I/O interface tests passing

### Milestone 3: Historical Validation

- [ ] Apollo 11 descent trajectory matches within tolerance
- [ ] Apollo 12 precision landing validated
- [ ] All historical tests passing

### Milestone 4: Performance Validated

- [ ] All benchmarks meet targets
- [ ] No heap allocations in critical paths
- [ ] Memory usage within limits

## Test Execution

### Local Testing

```bash
# Run all tests
cargo test

# Run with nextest (faster)
cargo nextest run

# Run specific test
cargo test test_name

# Run with coverage
cargo tarpaulin --out Html
```

### CI Testing

- [ ] All tests run on push
- [ ] Historical tests run on PR
- [ ] Benchmarks run on PR (with comparison)
- [ ] Coverage report generated

## Open Questions

1. **Tolerance for historical validation**: What's acceptable deviation from mission data?
2. **AGC simulator integration**: How to automate comparison with Virtual AGC?
3. **Test data format**: JSON, CSV, or binary for historical data?
4. **Property test strategy**: Which properties are most valuable to test?
5. **Benchmark baseline**: What hardware should be the benchmark reference?

## Next Steps

1. [ ] Define first batch of unit tests for core types
2. [ ] Set up test infrastructure (fixtures, helpers)
3. [ ] Research Apollo telemetry data sources
4. [ ] Define acceptance criteria for historical validation
5. [ ] Set up benchmark harness

## Notes

- Historical validation is critical for correctness
- Property tests catch edge cases unit tests miss
- Keep test data feature-gated to avoid bloating builds
- Document why each tolerance value was chosen
