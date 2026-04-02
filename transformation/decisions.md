# Architecture Decision Records (ADRs)

Track key architectural and design decisions for the AGC-to-Rust transformation.

## Format

Each decision follows this template:

```markdown
## ADR-XXX: Title

**Date**: YYYY-MM-DD  
**Status**: Proposed / Accepted / Superseded / Deprecated  
**Deciders**: [Who made this decision]

### Context

[What is the issue we're trying to solve? What constraints exist?]

### Decision

[What decision did we make?]

### Rationale

[Why did we choose this option?]

### Consequences

**Positive**:
- [Benefit 1]
- [Benefit 2]

**Negative**:
- [Cost/limitation 1]
- [Cost/limitation 2]

**Neutral**:
- [Impact 1]

### Alternatives Considered

1. **Option 1**: [Description] - Rejected because [reason]
2. **Option 2**: [Description] - Rejected because [reason]

### Related Decisions

- Supersedes: ADR-XXX
- Related to: ADR-XXX
```

---

## Decisions

### ADR-001: Spec-Driven Development Approach

**Date**: 2026-04-02  
**Status**: Accepted  
**Deciders**: User

#### Context

Transforming AGC assembly to Rust is complex. Need systematic approach to ensure correctness, completeness, and maintainability. Multiple transformation strategies possible: ad-hoc porting, test-driven development, spec-driven development.

#### Decision

Adopt spec-driven development workflow where detailed specifications are written before implementation. Each AGC component gets a spec file (routine, module, guidance algorithm, interrupt, or data structure) that defines requirements, API design, scaling, and test cases.

#### Rationale

1. **Specifications are easier to review than code** - Design errors caught early
2. **AI excels at spec-to-code translation** - Leverages AI strengths
3. **Specs become living documentation** - Single source of truth
4. **AGC domain knowledge captured** - Scaling, timing, edge cases documented
5. **Reproducible** - Same spec generates consistent implementations

#### Consequences

**Positive**:
- Design issues caught before coding
- Clear requirements reduce ambiguity
- Spec serves as test oracle
- Historical validation strategy defined upfront
- Easier onboarding (specs explain AGC behavior)

**Negative**:
- Upfront time investment in spec writing
- Specs must be kept in sync with implementation
- Risk of over-specification (analysis paralysis)

**Neutral**:
- Requires discipline to follow workflow
- Specs need review process

#### Alternatives Considered

1. **Ad-hoc porting**: Translate AGC line-by-line → Rejected: Hard to maintain, error-prone
2. **Test-driven**: Write tests first, then implement → Rejected: Test design requires same knowledge as spec
3. **Reference implementation**: Copy existing Rust AGC → Rejected: Want custom architecture

#### Related Decisions

- Informs ADR-002 (workspace structure)
- Informs ADR-003 (fixed-point strategy)

---

## Proposed Decisions

### ADR-002: Workspace Structure (DRAFT)

**Date**: TBD  
**Status**: Proposed  
**Deciders**: TBD

#### Context

Need to decide whether to use Rust workspace with multiple crates or single crate with modules. AGC has natural component boundaries (executive, guidance, navigation, I/O).

#### Options

1. **Workspace with multiple crates**:
   - agc-core, agc-memory, agc-executive, agc-guidance, etc.
   - Clear boundaries, parallel compilation
   - More complex dependency management

2. **Single crate with modules**:
   - Simpler structure, easier to start
   - Harder to enforce boundaries
   - Faster initial iteration

3. **Hybrid**: Start single crate, split into workspace later

#### Decision

TBD - Need to decide before creating Cargo.toml

---

### ADR-003: Fixed-Point Arithmetic Strategy (DRAFT)

**Date**: TBD  
**Status**: Proposed  
**Deciders**: TBD

#### Context

AGC uses 15-bit + sign fixed-point arithmetic with various scale factors. Need Rust representation that preserves scaling semantics while being ergonomic.

#### Options

1. **Use `fixed` crate**:
   - Mature library with good performance
   - May not match AGC scaling exactly

2. **Use `nalgebra` fixed-point**:
   - Integrated with linear algebra
   - Unknown performance

3. **Custom `Scaled<N, T>` type**:
   - Full control over behavior
   - Must implement everything ourselves
   - Can use const generics for compile-time checking

4. **Newtype wrappers around primitives**:
   - Simplest approach
   - Less type safety
   - Manual scaling management

#### Decision

TBD - Research needed on each option

---

### ADR-004: Emulator vs Reimplementation (DRAFT)

**Date**: TBD  
**Status**: Proposed  
**Deciders**: TBD

#### Context

From Apollo.md, two main approaches:
1. **Emulator**: Simulate AGC hardware, run original binary
2. **Reimplementation**: Translate algorithms to idiomatic Rust

#### Decision

TBD - Depends on project goals (education vs practical use)

---

## Decision Backlog

Questions that need ADRs:

1. **Error handling strategy**: Panic vs Result? Custom error types?
2. **Async/await**: Use async for task scheduling or stick with sync?
3. **No-std support**: Target embedded platforms or stay with std?
4. **Dependency policy**: When to add external crates?
5. **Test data management**: How to handle large historical datasets?
6. **WASM target**: Worth supporting for web demos?
7. **Interrupt model**: How to represent AGC interrupts in Rust?
8. **Bank switching**: How to model in Rust (not needed, but document why)?
9. **Historical accuracy vs idiomatic Rust**: Balance point?

## Notes

- Keep ADRs focused and actionable
- Every major architectural choice should have an ADR
- ADRs can be superseded but never deleted (preserve history)
- Reference ADRs in code comments when relevant
- Link ADRs to specs and issues
