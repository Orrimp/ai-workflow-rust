# Data Structure Spec: [STRUCTURE_NAME]

> Template for transforming AGC data structures, memory maps, and tables to Rust

## AGC Source Reference

**Structure Name**: `[NAME]` (e.g., `STATE_VECTOR`, `PHASE_TABLE`)  
**Source File**: `[FILE].agc`  
**Line Range**: [START]-[END]  
**Memory Type**: Erasable / Fixed / Channel  
**Address Range**: [Octal start]-[Octal end]

### Original AGC Definition

```assembly
# Paste relevant AGC data structure definition here
# Include comments from original source
```

## Purpose

What is this data structure used for?

**Used By**: [List modules/routines that access this structure]

**Lifetime**: [When is it created/destroyed? Always present?]

**Scope**: [Global/Module-local/Task-local]

## Structure

### Layout

AGC memory layout:

| Offset | Name | Type | Scaling | Purpose |
|--------|------|------|---------|---------|
| +0 | [Field 1] | Word/2-word | [Scale] | [Purpose] |
| +1 | [Field 2] | Word/2-word | [Scale] | [Purpose] |
| +2 | [Field 3] | Word/2-word | [Scale] | [Purpose] |

**Total Size**: [N] words ([N*16] bits)

### Field Details

For each field, provide detailed information:

#### Field: [NAME]

- **AGC Address**: [Octal]
- **Type**: [1-word/2-word/vector/matrix]
- **Scaling**: [Fixed-point scale factor]
- **Range**: [Min..Max in physical units]
- **Precision**: [Bits of precision / LSB value]
- **Units**: [Meters, m/s, radians, dimensionless, etc.]
- **Access Pattern**: [Read-only/Write-only/Read-write]
- **Update Rate**: [How often updated]

## Data Format

### Fixed-Point Representation

For each fixed-point field:

| Field | Integer Type | Fractional Bits | Scale Factor | Physical Range |
|-------|--------------|-----------------|--------------|----------------|
| [Field] | i16/i32 | [N] | 2^[-N] | [min..max] |

### Special Encodings

If any fields use special encodings (packed bits, flags, etc.):

| Field | Encoding | Bit Layout | Interpretation |
|-------|----------|------------|----------------|
| [Field] | [Type] | [Bits] | [How to decode] |

### Alignment

- **AGC alignment**: [Word-aligned, 2-word aligned, etc.]
- **Rust alignment**: [How to preserve in Rust]

## Access Patterns

### Readers

Who reads this data?

| Reader | Access Frequency | Critical Path? | Concurrent? |
|--------|------------------|----------------|-------------|
| [Module/Routine] | [Rate] | Yes/No | Yes/No |

### Writers

Who writes this data?

| Writer | Access Frequency | Critical Path? | Concurrent? |
|--------|------------------|----------------|-------------|
| [Module/Routine] | [Rate] | Yes/No | Yes/No |

### Concurrent Access

Is this structure accessed from multiple tasks/interrupts?

- **Concurrent readers**: [List]
- **Concurrent writers**: [List]
- **Synchronization needed**: Yes/No
- **AGC protection mechanism**: [INHINT/RELINT, priority levels, etc.]

## Rust Type Design

### Type Definition

```rust
/// [Documentation for the data structure]
/// 
/// # AGC Source
/// 
/// Original: [NAME] at addresses [range] in [file]
/// 
/// # Memory Layout
/// 
/// [Brief layout description]
/// 
/// # Scaling
/// 
/// [Brief scaling description]
#[derive(Debug, Clone, Copy)]
#[repr(C)]  // If layout must match AGC exactly
pub struct StructureName {
    /// [Field 1 documentation with scaling info]
    pub field1: Scaled<-28, i32>,  // Or appropriate type
    
    /// [Field 2 documentation with scaling info]
    pub field2: Scaled<-7, i16>,
    
    /// [Field 3 documentation]
    pub field3: Type3,
}
```

### Alternative Design: Newtype Wrappers

For simple structures, consider newtype wrappers:

```rust
/// [Documentation]
/// 
/// AGC: [address], scaling 2^[N]
#[derive(Debug, Clone, Copy, PartialEq)]
pub struct FieldName(Scaled<N, i16>);

impl FieldName {
    pub fn new(value: f64) -> Self {
        Self(Scaled::from_float(value))
    }
    
    pub fn as_float(&self) -> f64 {
        self.0.to_float()
    }
}
```

### Builder Pattern

If structure initialization is complex:

```rust
pub struct StructureNameBuilder {
    // Fields
}

impl StructureNameBuilder {
    pub fn new() -> Self { /* ... */ }
    
    pub fn field1(mut self, value: Type1) -> Self {
        self.field1 = value;
        self
    }
    
    pub fn build(self) -> StructureName {
        StructureName { /* validate and build */ }
    }
}
```

### Default Values

```rust
impl Default for StructureName {
    fn default() -> Self {
        Self {
            field1: Scaled::from_raw([AGC default value]),
            field2: Scaled::from_raw([AGC default value]),
            field3: [AGC default value],
        }
    }
}
```

## API Design

### Accessor Methods

```rust
impl StructureName {
    /// Get [field] in [physical units]
    pub fn field1(&self) -> f64 {
        self.field1.to_float()
    }
    
    /// Set [field] from [physical units]
    pub fn set_field1(&mut self, value: f64) {
        self.field1 = Scaled::from_float(value);
    }
    
    /// Get [field] as raw AGC value (for serialization/debugging)
    pub fn field1_raw(&self) -> i32 {
        self.field1.raw()
    }
}
```

### Conversion Methods

```rust
impl StructureName {
    /// Convert from AGC raw memory representation
    pub fn from_agc_words(words: &[u16]) -> Result<Self, DecodeError> {
        todo!()
    }
    
    /// Convert to AGC raw memory representation
    pub fn to_agc_words(&self) -> Vec<u16> {
        todo!()
    }
}
```

## Validation

### Invariants

Properties that must always hold:

1. **INV-1**: [e.g., "All fields within valid physical ranges"]
2. **INV-2**: [e.g., "State vector components are orthogonal"]
3. **INV-3**: [e.g., "Checksum matches (if applicable)"]

### Validation Methods

```rust
impl StructureName {
    /// Validate that all invariants hold
    pub fn validate(&self) -> Result<(), ValidationError> {
        // Check invariant 1
        if !self.field1_in_range() {
            return Err(ValidationError::Field1OutOfRange);
        }
        
        // Check invariant 2
        // ...
        
        Ok(())
    }
    
    fn field1_in_range(&self) -> bool {
        let value = self.field1.to_float();
        value >= MIN_FIELD1 && value <= MAX_FIELD1
    }
}
```

## Test Cases

### Construction Tests

```rust
#[test]
fn test_structure_default() {
    let s = StructureName::default();
    assert_eq!(s.field1.raw(), [expected AGC raw value]);
    assert_eq!(s.field2.raw(), [expected AGC raw value]);
}

#[test]
fn test_structure_builder() {
    let s = StructureNameBuilder::new()
        .field1([value])
        .field2([value])
        .build();
    
    assert_eq!(s.field1(), [expected physical value]);
}
```

### Scaling Tests

```rust
#[test]
fn test_field1_scaling() {
    let mut s = StructureName::default();
    
    // Set to known physical value
    s.set_field1(100.0);  // e.g., 100 meters
    
    // Verify raw AGC value is correct
    assert_eq!(s.field1_raw(), [expected AGC raw value]);
    
    // Verify round-trip
    assert!((s.field1() - 100.0).abs() < 1e-6);
}
```

### Serialization Tests

```rust
#[test]
fn test_agc_word_roundtrip() {
    let original = StructureName {
        field1: Scaled::from_raw([value]),
        field2: Scaled::from_raw([value]),
        field3: [value],
    };
    
    let words = original.to_agc_words();
    assert_eq!(words.len(), [expected word count]);
    
    let reconstructed = StructureName::from_agc_words(&words).unwrap();
    assert_eq!(reconstructed, original);
}
```

### Validation Tests

```rust
#[test]
fn test_validation_success() {
    let valid = StructureName::default();
    assert!(valid.validate().is_ok());
}

#[test]
fn test_validation_failure() {
    let mut invalid = StructureName::default();
    invalid.field1 = Scaled::from_raw(i32::MAX);  // Out of range
    assert!(invalid.validate().is_err());
}
```

### Property-Based Tests

```rust
proptest! {
    #[test]
    fn prop_agc_word_roundtrip(
        field1 in valid_field1_range(),
        field2 in valid_field2_range()
    ) {
        let original = StructureName {
            field1: Scaled::from_float(field1),
            field2: Scaled::from_float(field2),
            field3: Default::default(),
        };
        
        let words = original.to_agc_words();
        let reconstructed = StructureName::from_agc_words(&words).unwrap();
        
        // Assert close enough (account for fixed-point precision loss)
        prop_assert!((reconstructed.field1() - field1).abs() < TOLERANCE);
        prop_assert!((reconstructed.field2() - field2).abs() < TOLERANCE);
    }
}
```

## Historical Validation

If applicable, provide known values from Apollo missions:

### Apollo [X] Mission Data

**Event**: [e.g., "State vector at PDI"]  
**Source**: [Mission log, telemetry]

| Field | AGC Raw Value | Physical Value | Units |
|-------|---------------|----------------|-------|
| field1 | [octal] | [decimal] | [units] |
| field2 | [octal] | [decimal] | [units] |

**Validation Test**:

```rust
#[test]
fn test_apollo_[x]_state_vector() {
    let state = StructureName {
        field1: Scaled::from_raw([known AGC value]),
        field2: Scaled::from_raw([known AGC value]),
        // ...
    };
    
    // Verify physical interpretation matches mission data
    assert!((state.field1() - [known physical value]).abs() < TOLERANCE);
    assert!((state.field2() - [known physical value]).abs() < TOLERANCE);
}
```

## Synchronization

If concurrent access is possible:

### Thread Safety

```rust
// For shared state accessed by multiple tasks:
use std::sync::Arc;
use parking_lot::RwLock;

pub type SharedStructureName = Arc<RwLock<StructureName>>;
```

### Atomic Updates

If structure must be updated atomically:

```rust
impl StructureName {
    /// Atomically update multiple fields
    pub fn atomic_update<F>(&mut self, f: F)
    where
        F: FnOnce(&mut Self),
    {
        // AGC would use INHINT/RELINT here
        // Rust: use appropriate synchronization
        f(self);
    }
}
```

## Performance Considerations

### Memory Footprint

- **AGC size**: [N words = N*16 bits]
- **Rust size**: `std::mem::size_of::<StructureName>()` should be [expected bytes]
- **Alignment**: [bytes]

### Access Performance

- **Hot path?**: [Is this structure accessed in performance-critical code?]
- **Cache considerations**: [Any cache-line alignment needed?]
- **Stack vs heap**: [Should instances live on stack or heap?]

## Dependencies

### Type Dependencies

- [ ] `Scaled<N, T>` fixed-point type
- [ ] `Vector3<T>` for 3-vectors
- [ ] `Matrix3<T>` for 3x3 matrices
- [ ] Other: [list]

### Module Dependencies

- [ ] `memory` module for AGC address types
- [ ] `validation` module for error types
- [ ] Other: [list]

## Migration Notes

How to transition from AGC representation to Rust:

1. **Step 1**: [e.g., "Define Rust struct with correct layout"]
2. **Step 2**: [e.g., "Implement accessor methods"]
3. **Step 3**: [e.g., "Add validation logic"]
4. **Step 4**: [e.g., "Write serialization/deserialization"]
5. **Step 5**: [e.g., "Port reader/writer code to use new type"]

## Validation Criteria

Data structure is complete when:

- [ ] Type definition matches AGC layout
- [ ] All fields have correct scaling
- [ ] Accessor methods implemented
- [ ] Conversion methods implemented (if needed)
- [ ] Validation methods implemented
- [ ] All tests passing
- [ ] Historical validation passing (if applicable)
- [ ] Documentation complete
- [ ] Code review approved

## Related Specs

- **Reader specs**: [Modules that read this structure]
- **Writer specs**: [Modules that write this structure]
- **Type specs**: [Related data structure specs]

## Status

- [ ] Spec reviewed and approved
- [ ] Type defined
- [ ] Accessors implemented
- [ ] Tests written and passing
- [ ] Code reviewed
- [ ] Integrated with readers/writers

## Notes

Additional context or observations:

- 
