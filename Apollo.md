# Rewriting Apollo 11 AGC in Rust

A guide for reimplementing the Apollo 11 Guidance Computer software in Rust.

## Core Knowledge Required

### 1. AGC Architecture Understanding

- **16-bit word machine** with unusual addressing modes
- **Fixed and erasable memory** (ROM/RAM) architecture
- **Interpretive vs basic instructions** - dual instruction sets
- **Priority-based interrupt system** with restart protection
- **Display/Keyboard (DSKY) interface** protocol

### 2. Domain Knowledge

- **Orbital mechanics** (Kepler orbits, Lambert targeting, rendezvous)
- **Inertial navigation** (IMU integration, gimbal lock avoidance)
- **Kalman filtering** for state estimation
- **Quaternions** and rotation matrices
- **Fixed-point arithmetic** (AGC used scaled integers, not floating point)

### 3. Real-time Systems Concepts

- **Cooperative multitasking** with priority scheduling
- **Waitlist/job queue** management
- **Restart protection** (checkpointing mid-calculation)

## Recommended Rust Ecosystem

### Core Crates

#### Numerical Computing

- `nalgebra` - Linear algebra (vectors, matrices, quaternions)
- `simba` - SIMD-accelerated math
- `fixed` or `fixed-point` - Fixed-point arithmetic to match AGC behavior
- `num-traits` - Generic numeric traits

#### Simulation & Emulation

- `bitflags` - Hardware register modeling
- `nom` or `pest` - Parsing AGC assembly if you want to load original code
- `dashmap` - Concurrent data structures for multi-threaded state

#### no_std Support (if targeting embedded)

- `heapless` - Stack-based collections
- `arrayvec` - Fixed-capacity vectors
- `embedded-hal` - Hardware abstraction for peripheral access
- `cortex-m` or `riscv` - If running on actual microcontrollers

#### Testing & Validation

- `proptest` or `quickcheck` - Property-based testing for orbital mechanics
- `approx` or `float-cmp` - Floating-point comparison with tolerance
- `criterion` - Benchmarking for performance-critical guidance loops

## Architecture Decisions

### 1. Emulator vs Reimplementation

**Emulator**: Cycle-accurate AGC simulation (runs original .agc binaries)
- Pros: Faithful to original hardware, can run actual Apollo code
- Cons: Complex, requires deep hardware knowledge

**Reimplementation**: Modern Rust code implementing the same algorithms
- Pros: Cleaner code, easier to understand and extend
- Cons: Not bit-for-bit compatible with original

### 2. Structure Options

```rust
// Option A: Pure emulator
struct AGC {
    memory: [u16; 0x10000],
    registers: Registers,
    interrupt_mask: u16,
}

impl AGC {
    fn execute_instruction(&mut self) -> Result<()>;
    fn handle_interrupt(&mut self, vector: u16);
}
```

```rust
// Option B: Modular reimplementation
mod guidance {
    mod kalman_filter;
    mod lambert_targeting;
    mod powered_descent;
}

mod executive {
    mod scheduler;
    mod restart;
}

mod interpreter; // High-level interpretive language
```

### 3. Trait Design for Guidance Algorithms

```rust
trait GuidanceProgram {
    fn initialize(&mut self, state: &VehicleState) -> Result<()>;
    fn update(&mut self, dt: f64, state: &VehicleState) -> ControlOutput;
    fn can_restart(&self) -> bool;
}

struct PoweredDescent {
    target_position: Vector3,
    throttle_law: ThrottleLaw,
    // ...
}

impl GuidanceProgram for PoweredDescent {
    // Implementation
}
```

## Suggested Approach

### Phase 1: Core Infrastructure

1. AGC memory model and instruction decoder
2. Basic instruction execution engine
3. Interpretive language subset

**Deliverable**: Simple program executes basic math operations

### Phase 2: Navigation Foundation

1. State vector propagation (Encke method like AGC)
2. Coordinate transformations
3. IMU integration

**Deliverable**: Orbital propagator matches known test cases

### Phase 3: Guidance Programs

1. Start with simpler programs (P20 rendezvous)
2. Build up to P63/P64 powered descent
3. Validate against known Apollo 11 telemetry data

**Deliverable**: Recreate Apollo 11 descent trajectory

### Phase 4: Integration

1. DSKY interface (terminal or GUI)
2. Real-time execution
3. Telemetry/logging

**Deliverable**: Interactive AGC simulator

## Learning Resources

### Existing Rust Implementations

- `felipevb/ragc` - Study their emulator approach
- `K0R0VA/apollo-gateway-rs` - See architectural decisions

### Essential References

- **Virtual AGC project** (virtualagc.org) - Deep technical documentation
  - AGC assembly language reference
  - Hardware specifications
  - Original MIT documentation
  
- **Apollo 11 Flight Journal** - Mission timeline and guidance mode usage
  - When each program (P-code) was used
  - Actual telemetry data
  
- **MIT IL reports** - Original AGC design documents
  - R-567: AGC4 Basic Training Manual
  - E-2052: Guidance System Operations Plan
  
- **"Digital Apollo"** by David Mindell - Historical context
  - Design decisions and trade-offs
  - Human-computer interaction philosophy

### Technical Papers

- "Apollo Guidance Computer Architecture and Operation" - Eldon C. Hall
- "The Apollo Guidance Computer: Architecture and Operation" - Frank O'Brien
- NASA Technical Reports on Kalman filtering and guidance equations

## Validation Strategy

### Test Oracles

**Orbital mechanics:**
- Compare with `poliastro` (Python) or `orekit` (Java)
- Known solutions from celestial mechanics textbooks
- Two-body problem analytical solutions

**AGC behavior:**
- Run same inputs through Virtual AGC emulator
- Compare instruction-by-instruction execution
- Verify memory state at checkpoints

**Historical data:**
- Apollo 11 telemetry logs from NASA archives
- Flight-proven trajectories
- Mission timeline events

### Test Categories

```rust
#[cfg(test)]
mod tests {
    // Unit tests for individual functions
    #[test]
    fn test_two_body_propagation() { }
    
    // Integration tests for subsystems
    #[test]
    fn test_kalman_filter_update() { }
    
    // Property-based tests
    #[test]
    fn prop_orbital_energy_conserved() { }
    
    // Historical validation
    #[test]
    fn test_apollo_11_descent_trajectory() { }
}
```

## Development Workflow Considerations

### Should You Create a Specialized AGC-to-Rust Skill or Agent?

There's a genuine trade-off between specialized tooling and simplicity.

#### When a Specialized Skill/Agent Makes Sense

**YES, create one if:**

1. **You're doing systematic transformation** of many AGC modules
   - Converting the entire Comanche055 or Luminary099 codebase
   - Batch processing multiple .agc files
   - Need repeatable transformation patterns

2. **You want bundled reference materials**
   - AGC instruction set quick reference
   - Common transformation patterns (e.g., `DXCH → mem::swap`, `DCA → load_double`)
   - Scaling factor lookup tables
   - Memory map and register conventions

3. **You need domain-specific validation**
   - Verify fixed-point scaling matches original
   - Check interrupt priorities preserved
   - Validate restart protection logic
   - Compare output semantics against Virtual AGC

**Example skill structure:**
```
.github/skills/apollo-to-rust/
├── SKILL.md                    # Transformation workflow
├── agc-instruction-set.md      # Quick reference
├── transformation-patterns.md  # Common AGC→Rust idioms
└── scaling-tables.md           # Fixed-point scale factors
```

#### When to Skip It

**NO, use existing rust-development skill if:**

1. **One-time or learning project**
   - Just exploring AGC architecture
   - Building understanding before deciding on approach
   - Experimental transformation

2. **High-level reimplementation**
   - Not translating line-by-line
   - Implementing algorithms in idiomatic Rust
   - Using this guide as conceptual reference only

3. **Maintenance overhead isn't worth it**
   - Adds another file to keep updated
   - The existing rust-development skill + this guide covers most needs

### Recommendation

**Start without a specialized skill.** Here's why:

1. **Your current setup may already be sufficient:**
   - This Apollo.md provides architectural knowledge
   - rust-development skill handles Rust implementation
   - rust-testing skill covers validation

2. **Create the skill later if you notice:**
   - Repeating the same transformation questions
   - Looking up the same AGC instructions repeatedly
   - Needing the same validation checklist multiple times

3. **A specialized agent might be overkill**, but consider it if:
   - You want to isolate AGC analysis (read-only exploration) from Rust generation (code writing)
   - You're building a translation pipeline where different stages have different tool needs

### Lightweight Alternative

Instead of a full skill, **enhance your AGENTS.md** with AGC-specific guidance:

```markdown
## Apollo AGC Transformation Notes

When converting AGC assembly to Rust:
- AGC uses 1's complement arithmetic, not 2's complement
- All arithmetic is scaled fixed-point (track scaling metadata carefully)
- EXTEND prefix doubles the following instruction's index register range
- Interrupts: preserve priority order and restart semantics
- DXCH exchanges double-precision values (consider mem::swap)
- Cross-reference with Virtual AGC yaYUL assembler for edge cases
- Test against known trajectories from Apollo mission data
```

This gives you specialized guidance without the overhead of a separate skill file.

**Bottom line:** Wait until you've done a few transformations manually. If you find yourself copy-pasting the same reference info or asking the same questions, *then* extract it into a skill. Don't over-engineer upfront.

## Implementation Considerations

### Memory Management

The AGC had strict memory constraints:
- 2048 words erasable memory (RAM)
- 36,864 words fixed memory (ROM)

Consider using:
```rust
const ERASABLE_SIZE: usize = 2048;
const FIXED_SIZE: usize = 36864;

struct Memory {
    erasable: [u16; ERASABLE_SIZE],
    fixed: [u16; FIXED_SIZE],
}
```

### Fixed-Point Arithmetic

AGC used scaled integers for all calculations:
```rust
// Example: Represent velocities in meters/second scaled by 2^7
#[derive(Clone, Copy)]
struct Velocity(i32); // Internal scaling: 1 unit = 1/128 m/s

impl Velocity {
    fn from_mps(mps: f64) -> Self {
        Self((mps * 128.0) as i32)
    }
    
    fn to_mps(self) -> f64 {
        self.0 as f64 / 128.0
    }
}
```

### Interrupt Handling

AGC had 11 interrupt vectors with priorities:
```rust
enum Interrupt {
    T6RUPT,  // TIME6 overflow
    T5RUPT,  // TIME5 overflow
    T3RUPT,  // TIME3 overflow
    T4RUPT,  // TIME4 overflow
    KEYRUPT, // Keyboard
    UPRUPT,  // Uplink
    // ...
}

impl AGC {
    fn service_interrupts(&mut self) {
        // Handle by priority
    }
}
```

## Milestones

### Milestone 1: Basic Emulator
- [ ] Memory system
- [ ] Instruction decoder
- [ ] Basic arithmetic instructions
- [ ] Simple test programs run

### Milestone 2: Interpretive Language
- [ ] Interpretive instruction set
- [ ] Vector/matrix operations
- [ ] Transcendental functions (sin, cos, sqrt)

### Milestone 3: Navigation Core
- [ ] State vector representation
- [ ] Orbital propagation
- [ ] Coordinate transformations

### Milestone 4: Guidance Algorithms
- [ ] P20 (Rendezvous navigation)
- [ ] P63 (Braking phase)
- [ ] P64 (Approach phase)
- [ ] P66 (Landing phase)

### Milestone 5: Full System
- [ ] DSKY interface
- [ ] All major programs (P-codes)
- [ ] Validate against Apollo 11 mission data
- [ ] Performance optimization

## Getting Started

1. **Clone the existing Rust implementations** to study
2. **Read Virtual AGC documentation** for hardware understanding
3. **Start small**: Implement memory and basic instructions
4. **Build incrementally**: Add complexity one subsystem at a time
5. **Test continuously**: Validate against known correct behavior

## Next Steps

Consider which approach fits your goals:
- **Educational**: Reimplementation with modern Rust idioms
- **Historical accuracy**: Cycle-accurate emulator
- **Practical**: Hybrid approach with accurate algorithms but modern structure

The choice will determine your architecture and dependency choices.
