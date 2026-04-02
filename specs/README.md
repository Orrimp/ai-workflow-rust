# AGC Assembly to Rust Transformation Specs

This directory contains specification templates for systematically transforming Apollo Guidance Computer (AGC) assembly code into idiomatic Rust.

## Spec-Driven Transformation Workflow

1. **Choose the appropriate template** based on what you're transforming
2. **Fill out the spec** with AGC source reference, requirements, and API design
3. **Review the spec** for completeness and correctness (cheaper than reviewing code)
4. **Use AI to generate** implementation using `/rust-development` skill
5. **Validate** with tests generated from spec test cases
6. **Iterate** on spec if design issues emerge, then regenerate

## Available Templates

| Template | Use For | Example AGC Components |
|----------|---------|----------------------|
| [routine-spec.md](routine-spec.md) | Individual subroutines/procedures | `NEWMODEX`, `BANKCALL`, `WAITLIST` |
| [module-spec.md](module-spec.md) | Complete AGC modules | `PINBALL`, `SERVICER`, `EXECUTIVE` |
| [guidance-algorithm-spec.md](guidance-algorithm-spec.md) | Guidance programs | P63 (braking), P64 (approach), P66 (ROD) |
| [interrupt-spec.md](interrupt-spec.md) | Interrupt handlers | `T6RUPT`, `T3RUPT`, `KEYRUPT` |
| [data-structure-spec.md](data-structure-spec.md) | Memory structures, tables | Erasable memory, vector tables, phase tables |

## AGC Architecture Quick Reference

### Memory Organization
- **Erasable**: 2K words RAM (addresses 0000-1777 octal)
- **Fixed**: 36K words ROM (banks 02-43 octal)
- **Channels**: I/O mapped hardware interfaces

### Instruction Set
- **Basic**: `TC`, `CCS`, `INDEX`, `XCH`, `CS`, `TS`, `AD`, `MASK`
- **Extended** (EXTEND prefix): `DCA`, `DCS`, `INDEX`, `SU`, `BZMF`, `MP`, `DV`, `BZF`, `MSU`, `QXCH`, `AUG`, `DIM`
- **Double precision**: `DXCH`, `DCA`, `DCS`, `DDOUBL`, `DAS`, `LXCH`, `INCR`, `ADS`

### Common Patterns
- **Bank switching**: `TC BANKCALL` + `CADR <routine>`
- **Interrupts**: Priority-based, triggered by hardware channels
- **Waitlisting**: Deferred task scheduling via `WAITLIST`, `TASKOVER`
- **Display/Keyboard**: DSKY interface via `PINBALL`, verb/noun system
- **Restart protection**: Critical regions with `INHINT`/`RELINT`

### Scaling Conventions
All arithmetic is scaled fixed-point:
- Positions: meters scaled at 2^-28
- Velocities: m/s scaled at 2^-7  
- Time: centiseconds scaled at 2^-14
- Angles: half-revolutions or radians
- **Always track scale factors in specs!**

## Transformation Strategy

### Bottom-Up Approach
1. Start with **data structures** (erasable variables, constants)
2. Transform **utility routines** (math, scaling, conversions)
3. Build **core services** (executive, waitlist, display)
4. Implement **guidance algorithms** (using established foundations)
5. Add **interrupt handlers** last (depend on everything else)

### Top-Down Approach
1. Define **guidance program trait** API first
2. Stub out **algorithm interfaces** (P63, P64, P66, P67)
3. Identify **required services** and mock them
4. Fill in **implementation** iteratively
5. Replace **mocks** with real implementations

### Hybrid Approach (Recommended)
1. Spec **high-level guidance program API** (top-down)
2. Implement **fixed-point arithmetic and core types** (bottom-up)
3. Transform **one complete guidance program** end-to-end (validates both)
4. Repeat for remaining programs using established patterns

## Using Specs with AI

### Generation Prompts

After filling out a spec, use prompts like:

```
I have a spec for transforming AGC [routine/module/algorithm] to Rust.
Please generate:
1. Implementation following rust-development patterns
2. Unit tests covering all spec test cases
3. Integration test demonstrating the spec's example usage
4. Documentation with AGC source cross-references

Follow AGENTS.md guidelines and Apollo.md transformation notes.
```

### Validation Prompts

After generation:

```
@Rust Code Review

Review this implementation against its spec:
- Does it satisfy all requirements?
- Are invariants preserved?
- Do tests cover all spec test cases?
- Is the scaling correct per AGC conventions?
- Are there ownership/safety issues?
```

### Iteration Prompts

If issues emerge:

```
The spec for [component] needs revision because [reason].
Update the spec to [changes], then regenerate implementation.
Preserve existing tests that are still valid.
```

## Spec Quality Checklist

Before using a spec for generation:

- [ ] AGC source file and line range referenced
- [ ] All erasable variables and their addresses listed
- [ ] Scaling factors documented for all fixed-point values
- [ ] Input/output preconditions and postconditions stated
- [ ] Edge cases and error handling specified
- [ ] At least 3 test cases with expected values
- [ ] Rust API signature designed (types, ownership, lifetimes)
- [ ] Invariants explicitly stated
- [ ] Alignment with existing Rust modules noted

## Tips

### AGC Source References
Always reference original source:
```
Source: Luminary099/LUNAR_LANDING_GUIDANCE_EQUATIONS.agc
Lines: 234-456
```

### Scaling Documentation
Be explicit:
```
Input: altitude in meters, scaled 2^-28 (AGC format)
Output: velocity in m/s, scaled 2^-7 (AGC format)
```

### Test Oracle Strategy
Use known Apollo mission data:
- Apollo 11 descent trajectory
- Apollo 12 precision landing
- Apollo 13 abort sequence
- Apollo 14-17 mission profiles

### Rust API Design Philosophy
- Prefer newtype wrappers for scaled values: `Meters(i32)`, `Velocity(i16)`
- Use const generics for scale factors where practical: `Scaled<-28>`
- Make I/O and time dependencies explicit parameters
- Keep AGC memory model separate from algorithm logic
- Use traits for guidance program polymorphism

## Example Workflow

```bash
# 1. Copy template
cp specs/guidance-algorithm-spec.md specs/p63-braking-spec.md

# 2. Fill out spec (refer to Luminary099/P63.agc)
# Edit specs/p63-braking-spec.md...

# 3. Generate implementation
# In VS Code chat:
# "@Rust Implementer implement specs/p63-braking-spec.md"

# 4. Run tests
cargo test p63

# 5. Review
# "@Rust Code Review check src/guidance/p63.rs against specs/p63-braking-spec.md"

# 6. Iterate if needed
# Update spec, regenerate, retest
```

## Related Documentation

- [Apollo.md](../Apollo.md) - Comprehensive AGC-to-Rust transformation guide
- [AGENTS.md](../.github/AGENTS.md) - Rust coding conventions for this workspace
- [WORKSPACE.md](../WORKSPACE.md) - Development environment setup

## References

- **AGC Source**: [chrislgarry/Apollo-11](https://github.com/chrislgarry/Apollo-11)
- **Virtual AGC**: [ibiblio.org/apollo](https://www.ibiblio.org/apollo/)
- **AGC Assembly Manual**: Virtual AGC documentation
- **Existing Rust AGC**: [felipevb/ragc](https://github.com/felipevb/ragc), [K0R0VA/apollo-gateway-rs](https://github.com/K0R0VA/apollo-gateway-rs)
