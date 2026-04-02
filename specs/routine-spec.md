# Routine Transformation Spec: [ROUTINE_NAME]

> Template for transforming individual AGC subroutines/procedures to Rust

## AGC Source Reference

**Source File**: `[AGC_SOURCE_FILE].agc` (e.g., `Luminary099/EXECUTIVE.agc`)  
**Line Range**: [START]-[END]  
**Original Routine Name**: `[AGC_ROUTINE_NAME]`

### Original AGC Code

```assembly
# Paste relevant AGC assembly here for reference
# Include comments from original source
```

## Purpose

Brief description of what this routine does in the AGC system.

**Called By**: [List other routines that call this one]  
**Calls**: [List routines this one calls]

## Requirements

List functional requirements extracted from AGC code and documentation:

1. **Primary function**: [What does it accomplish?]
2. **Inputs**: [What data does it consume?]
3. **Outputs**: [What data does it produce?]
4. **Side effects**: [What erasable memory does it modify?]
5. **Timing constraints**: [Any real-time requirements?]
6. **Bank switching**: [Does it require BANKCALL or cross-bank access?]

## Erasable Memory

List all erasable variables accessed by this routine:

| AGC Address | Variable Name | Type | Scaling | Purpose |
|-------------|---------------|------|---------|---------|
| E.g., 0034 | `VPRED` | 2-word vector | 2^-7 m/s | Predicted velocity |
| | | | | |

## Constants

List all fixed memory constants used:

| Name | Value | Scaling | Purpose |
|------|-------|---------|---------|
| | | | |

## Scaling

Document all fixed-point scaling factors used:

- **Input scaling**: [e.g., position in meters at 2^-28]
- **Output scaling**: [e.g., velocity in m/s at 2^-7]
- **Internal scaling**: [any scaling conversions performed]

## Rust API Design

### Function Signature

```rust
/// [Documentation: what this function does]
/// 
/// # Arguments
/// 
/// * `param1` - [description with scaling if applicable]
/// * `param2` - [description]
/// 
/// # Returns
/// 
/// [description with scaling if applicable]
/// 
/// # Errors
/// 
/// [when does this return Err?]
/// 
/// # AGC Equivalent
/// 
/// Original: `[AGC_ROUTINE_NAME]` in [file]:[lines]
pub fn routine_name(
    param1: Type1,
    param2: Type2,
) -> Result<ReturnType, ErrorType> {
    todo!()
}
```

### Alternative Designs Considered

If applicable, list alternative API designs and why they were rejected:

1. **Option 1**: [Description] - Rejected because [reason]
2. **Option 2**: [Description] - Chosen because [reason]

## Invariants

List properties that must always hold:

1. [Invariant 1: e.g., "Output velocity is always in valid range"]
2. [Invariant 2: e.g., "Never panics on valid input"]
3. [Invariant 3: e.g., "Preserves scaling factors correctly"]

## Edge Cases

Document how edge cases should be handled:

| Case | AGC Behavior | Rust Behavior |
|------|--------------|---------------|
| Division by zero | [How AGC handles it] | [How Rust should handle it] |
| Overflow | [How AGC handles it] | [How Rust should handle it] |
| Invalid input | [How AGC handles it] | [How Rust should handle it] |

## Test Cases

### Unit Tests

Provide at least 3 test cases with concrete values:

#### Test 1: [Description of test case]

```rust
#[test]
fn test_routine_name_case1() {
    let input1 = [concrete value];
    let input2 = [concrete value];
    let expected = [concrete value];
    
    let result = routine_name(input1, input2).unwrap();
    
    assert_eq!(result, expected);
    // Or use appropriate floating-point comparison for fixed-point:
    // assert!((result - expected).abs() < epsilon);
}
```

#### Test 2: [Edge case description]

```rust
#[test]
fn test_routine_name_edge_case() {
    // Test edge case behavior
}
```

#### Test 3: [Error case description]

```rust
#[test]
fn test_routine_name_error_case() {
    // Test error handling
}
```

### Property-Based Tests

If applicable, describe properties to test with `proptest`:

```rust
proptest! {
    #[test]
    fn prop_routine_preserves_invariant(input: ValidInput) {
        let result = routine_name(input).unwrap();
        // Assert invariant holds
    }
}
```

## Historical Validation

If possible, identify Apollo mission data to validate against:

- **Mission**: Apollo [X]  
- **Event**: [e.g., "Lunar descent at T+12:34"]  
- **Known inputs**: [from mission logs]  
- **Expected outputs**: [from mission logs]  
- **Tolerance**: [acceptable deviation]

## Dependencies

List other Rust modules/types this routine depends on:

- [ ] `types::Scaled<N>` for fixed-point arithmetic
- [ ] `memory::Erasable` for AGC memory access
- [ ] `executive::schedule_task` for task scheduling
- [ ] Other: [list any other dependencies]

## Implementation Notes

Any AGC-specific quirks or Rust translation challenges:

1. **AGC quirk**: [e.g., "Uses 1's complement arithmetic"]  
   **Rust approach**: [How to handle it]

2. **Bank switching**: [If applicable, how to handle cross-bank calls]

3. **Interrupt safety**: [If this routine is called from interrupts]

## Validation Criteria

How to verify the Rust implementation is correct:

- [ ] All unit tests pass
- [ ] Property-based tests pass (if applicable)
- [ ] Clippy clean with `-- -D warnings`
- [ ] Historical validation test passes (if applicable)
- [ ] Code review confirms scaling correctness
- [ ] No panics on valid inputs
- [ ] Matches AGC behavior on documented test cases

## Related Specs

Link to related transformation specs:

- [Module spec that contains this routine]
- [Specs for routines this one calls]
- [Specs for routines that call this one]

## Status

- [ ] Spec reviewed and approved
- [ ] Implementation generated
- [ ] Tests written and passing
- [ ] Code reviewed
- [ ] Historical validation complete (if applicable)
- [ ] Integrated into module

## Notes

Any additional notes, questions, or TODOs:

- 
