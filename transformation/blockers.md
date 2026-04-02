# Blockers and Open Questions

Track current blockers, unresolved questions, and dependencies that are preventing progress.

## Active Blockers

_No active blockers yet_

## Open Questions

### High Priority

#### Q1: Fixed-Point Library Choice

**Question**: Which fixed-point arithmetic library should we use?

**Options**:
1. `fixed` crate - Mature, performant
2. `nalgebra` fixed-point support
3. Custom `Scaled<N, T>` with const generics
4. Newtype wrappers around primitives

**Blocking**:
- Core types implementation
- All specs that use fixed-point arithmetic

**Next Steps**:
- Research each library's performance characteristics
- Test AGC scaling factor representation in each
- Create ADR-003 with decision

**Related**: [decisions.md ADR-003](decisions.md#adr-003-fixed-point-arithmetic-strategy-draft)

---

#### Q2: Workspace vs Single Crate

**Question**: Should we use a multi-crate workspace or single crate?

**Blocking**:
- Initial project setup
- Cargo.toml creation
- Module organization

**Next Steps**:
- Consider project complexity
- Evaluate compilation time trade-offs
- Create ADR-002 with decision

**Related**: [decisions.md ADR-002](decisions.md#adr-002-workspace-structure-draft)

---

#### Q3: Historical Validation Data Sources

**Question**: Where can we get Apollo mission telemetry data for validation?

**Blocking**:
- Historical validation tests
- Test oracle implementation
- Validation milestone completion

**Potential Sources**:
- NASA Technical Reports Server
- Apollo Lunar Surface Journal
- Virtual AGC project data
- Academic papers on Apollo guidance

**Next Steps**:
- Research data availability
- Contact Virtual AGC project
- Determine data format needs

**Related**: [validation.md Historical Validation Tests](validation.md#historical-validation-tests)

---

### Medium Priority

#### Q4: AGC Simulator Integration

**Question**: How should we integrate with Virtual AGC for reference comparisons?

**Options**:
1. Run Virtual AGC in parallel, compare outputs
2. Extract test vectors from Virtual AGC runs
3. Manual validation only
4. Build adapter interface

**Blocking**:
- Automated validation strategy
- CI/CD integration tests

**Next Steps**:
- Test Virtual AGC locally
- Assess automation feasibility
- Document in validation.md

---

#### Q5: Performance Baseline

**Question**: What hardware should be the benchmark reference?

**Considerations**:
- CI runners may have different specs
- Need consistent baseline for regression detection
- Should simulate various platforms?

**Blocking**:
- Benchmark targets in optimizations.md
- Performance regression detection

**Next Steps**:
- Define standard benchmark environment
- Document in optimizations.md

---

#### Q6: WASM Support Priority

**Question**: Should we prioritize WASM compilation for web demos?

**Trade-offs**:
- **Pro**: Great for visualization and education
- **Con**: Adds complexity, may limit dependencies
- **Con**: Performance characteristics differ

**Blocking**:
- Nothing critical (can add later)

**Next Steps**:
- Defer until core functionality works
- Revisit if demo/educational use becomes priority

---

### Low Priority

#### Q7: Documentation Format

**Question**: Use rustdoc only, or add mdBook for guides?

**Options**:
1. Rustdoc only - Simpler, standard
2. mdBook - Better for narrative documentation
3. Both - Rustdoc for API, mdBook for guides

**Blocking**:
- Nothing immediate

**Next Steps**:
- Start with rustdoc
- Add mdBook if narrative docs become substantial

---

#### Q8: License Choice

**Question**: What license should the project use?

**Considerations**:
- AGC code is public domain
- Want permissive license for derivative work
- MIT, Apache-2.0, or dual-license?

**Blocking**:
- Public release
- Dependency compatibility checks

**Next Steps**:
- Research AGC code licensing
- Choose permissive license
- Add LICENSE file

---

## Resolved Questions

_No questions resolved yet_

## Dependency Tracking

Track external dependencies blocking progress:

| Dependency | Type | Status | ETA | Blocking |
|------------|------|--------|-----|----------|
| _No external dependencies yet_ | | | | |

## Escalation

If a blocker cannot be resolved:

1. **Document**: Add to this file with full context
2. **Research**: Allocate time to investigate options
3. **Decide**: Make ADR even with incomplete information
4. **Timebox**: Set deadline for decision
5. **Move forward**: Choose best available option and iterate

## Notes

- Review blockers weekly
- Prioritize resolving high-priority questions
- Don't let perfect be enemy of good
- Document decisions even if uncertain
- Revisit decisions as new information emerges
