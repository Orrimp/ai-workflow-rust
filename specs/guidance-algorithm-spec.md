# Guidance Algorithm Spec: [PROGRAM_NAME]

> Template for transforming AGC guidance programs (P63, P64, P66, P67, etc.)

## AGC Source Reference

**Program Number**: P[XX]  
**Source File(s)**:
- `[PRIMARY_FILE].agc`
- `[RELATED_FILES].agc`

**Line Range**: [START]-[END]  
**Job Name**: `[AGC_JOB_NAME]` (if applicable)  
**Bank(s)**: [Bank numbers]

## Program Overview

**Purpose**: [What does this guidance program accomplish?]

**Phase**: [Mission phase: e.g., "Lunar descent braking phase"]

**Activation**:
- **Trigger**: [How is this program initiated?]
- **Entry Verb/Noun**: [DSKY interface]
- **Preconditions**: [What must be true before this program starts?]

**Termination**:
- **Normal completion**: [How does the program end successfully?]
- **Abort conditions**: [What causes early termination?]
- **Next program**: [What program follows this one?]

## Guidance Algorithm Description

### High-Level Algorithm

Describe the algorithm in plain language:

1. [Step 1 of algorithm]
2. [Step 2 of algorithm]
3. [Step 3 of algorithm]

### Mathematical Model

If applicable, provide the mathematical equations:

```
Equation 1: ẍ = f(x, v, t, θ)
Equation 2: v̇ = ...
```

**Reference Documents**:
- [NASA technical report citations]
- [Relevant sections of AGC documentation]

### Control Loop

**Update Rate**: [How often does the algorithm run? e.g., "Every 2 seconds"]  
**Integration Method**: [e.g., "Euler integration", "RK4"]  
**Convergence Criteria**: [When is the algorithm considered converged?]

## State Variables

### Input State

Variables read by this guidance program:

| AGC Address | Name | Type | Scaling | Purpose | Source |
|-------------|------|------|---------|---------|--------|
| | | | | | |

### Output State

Variables written by this guidance program:

| AGC Address | Name | Type | Scaling | Purpose | Consumer |
|-------------|------|------|---------|---------|----------|
| | | | | | |

### Internal State

Program-local variables:

| AGC Address | Name | Type | Scaling | Purpose | Lifetime |
|-------------|------|------|---------|---------|----------|
| | | | | | |

## External Dependencies

### Services Used

| Service | Purpose | Critical? |
|---------|---------|-----------|
| WAITLIST | Task scheduling | Yes |
| IMU | Inertial measurement | Yes |
| SERVICER | Display updates | No |
| THRUST | Engine control | Yes |

### Other Programs

| Program | Relationship | Transition |
|---------|--------------|------------|
| P[XX] | [Predecessor/successor/parallel] | [How transition occurs] |

## Guidance Equations

### Thrust Vector Calculation

```
AGC equation:
[Original AGC algorithm from comments/documentation]

Rust equivalent:
[How this translates to Rust code]
```

### Steering Commands

```
Roll: [equation]
Pitch: [equation]
Yaw: [equation]
```

### Throttle Control

```
[Throttle algorithm]
```

## Rust API Design

### Trait Definition

```rust
/// Trait for AGC guidance programs
/// 
/// Guidance programs implement this trait to provide a uniform
/// interface for the executive scheduler.
pub trait GuidanceProgram {
    /// Program initialization
    fn initialize(&mut self, state: &VehicleState) -> Result<(), GuidanceError>;
    
    /// Single guidance cycle update
    fn update(&mut self, dt: Duration, state: &VehicleState) -> Result<GuidanceOutput, GuidanceError>;
    
    /// Check if program should terminate
    fn should_terminate(&self) -> bool;
    
    /// Program number for identification
    fn program_number(&self) -> u8;
}
```

### Program Implementation

```rust
/// [Program description]
/// 
/// # AGC Source
/// 
/// Original: P[XX] in [source files]
/// 
/// # Algorithm
/// 
/// [Brief algorithm description]
/// 
/// # Activation
/// 
/// [How to activate this program]
pub struct P[XX] {
    // Internal state
    state: ProgramState,
    // Configuration parameters
    config: P[XX]Config,
}

impl P[XX] {
    /// Create new P[XX] guidance program
    pub fn new(config: P[XX]Config) -> Self {
        todo!()
    }
}

impl GuidanceProgram for P[XX] {
    fn initialize(&mut self, state: &VehicleState) -> Result<(), GuidanceError> {
        todo!()
    }
    
    fn update(&mut self, dt: Duration, state: &VehicleState) -> Result<GuidanceOutput, GuidanceError> {
        todo!()
    }
    
    fn should_terminate(&self) -> bool {
        todo!()
    }
    
    fn program_number(&self) -> u8 {
        [XX]
    }
}
```

### Configuration

```rust
/// Configuration parameters for P[XX]
#[derive(Debug, Clone)]
pub struct P[XX]Config {
    /// [Parameter 1 description]
    pub param1: Type1,
    /// [Parameter 2 description]
    pub param2: Type2,
}

impl Default for P[XX]Config {
    fn default() -> Self {
        Self {
            param1: [default from AGC constants],
            param2: [default from AGC constants],
        }
    }
}
```

### Types

```rust
/// Vehicle state snapshot
pub struct VehicleState {
    pub position: Vector3<Meters>,
    pub velocity: Vector3<MetersPerSecond>,
    pub attitude: Quaternion,
    pub time: Time,
}

/// Guidance output
pub struct GuidanceOutput {
    pub thrust_vector: Vector3<f64>,
    pub throttle: Throttle,
    pub attitude_cmd: Quaternion,
}

/// Guidance errors
#[derive(Debug, thiserror::Error)]
pub enum GuidanceError {
    #[error("convergence failure: {0}")]
    ConvergenceFailed(String),
    #[error("invalid state: {0}")]
    InvalidState(String),
    #[error("abort condition: {0}")]
    AbortCondition(String),
}
```

## Algorithm Implementation Details

### Initialization Phase

What happens when the program starts?

```rust
fn initialize(&mut self, state: &VehicleState) -> Result<(), GuidanceError> {
    // 1. [Initialize step 1]
    // 2. [Initialize step 2]
    // 3. [Initialize step 3]
    todo!()
}
```

### Update Cycle

What happens each guidance cycle?

```rust
fn update(&mut self, dt: Duration, state: &VehicleState) -> Result<GuidanceOutput, GuidanceError> {
    // 1. [Update step 1]
    // 2. [Update step 2]
    // 3. [Compute outputs]
    todo!()
}
```

### Termination Logic

When does the program end?

```rust
fn should_terminate(&self) -> bool {
    // Normal termination conditions:
    // - [Condition 1]
    // - [Condition 2]
    
    // Abort conditions:
    // - [Condition 3]
    // - [Condition 4]
    
    todo!()
}
```

## Constants

Program-specific constants from AGC:

| AGC Name | Value | Rust Constant | Scaling | Purpose |
|----------|-------|---------------|---------|---------|
| | | | | |

## Invariants

Properties that must always hold during program execution:

1. **INV-1**: [e.g., "Thrust vector magnitude ≤ 1.0"]
2. **INV-2**: [e.g., "Throttle in range [0.1, 1.0]"]
3. **INV-3**: [e.g., "Update rate is constant"]

## Test Cases

### Unit Tests

#### Test 1: Initialization

```rust
#[test]
fn test_p[xx]_initialization() {
    let config = P[XX]Config::default();
    let mut program = P[XX]::new(config);
    
    let initial_state = VehicleState {
        // Known initial conditions
    };
    
    program.initialize(&initial_state).unwrap();
    
    // Verify initialization succeeded
}
```

#### Test 2: Single Update Cycle

```rust
#[test]
fn test_p[xx]_update_cycle() {
    // Test one guidance cycle with known inputs/outputs
}
```

#### Test 3: Termination Detection

```rust
#[test]
fn test_p[xx]_termination() {
    // Test that program correctly detects termination conditions
}
```

### Integration Tests

#### Test 4: Complete Mission Segment

```rust
#[test]
fn test_p[xx]_complete_segment() {
    // Simulate entire program from start to termination
    let mut program = P[XX]::new(P[XX]Config::default());
    let mut state = initial_state();
    
    program.initialize(&state).unwrap();
    
    while !program.should_terminate() {
        let output = program.update(Duration::from_secs(2), &state).unwrap();
        // Update state based on output
        // Verify constraints maintained
    }
    
    // Verify program reached correct termination state
}
```

## Historical Validation

### Apollo Mission Data

**Mission**: Apollo [X]  
**Event**: [e.g., "Lunar descent from 50,000 ft to PDI"]

**Known Trajectory Points**:

| Time | Altitude | Velocity | Thrust | Attitude |
|------|----------|----------|--------|----------|
| T+0:00 | [m] | [m/s] | [%] | [deg] |
| T+0:30 | [m] | [m/s] | [%] | [deg] |
| T+1:00 | [m] | [m/s] | [%] | [deg] |

**Validation Test**:

```rust
#[test]
fn test_apollo_[x]_[event]_trajectory() {
    // Initialize program with Apollo [X] initial conditions
    // Run program through timeline
    // Compare outputs to known trajectory at each waypoint
    // Assert within acceptable tolerance
}
```

**Data Sources**:
- [Mission log references]
- [NASA technical reports]
- [Apollo Lunar Surface Journal]

**Acceptable Tolerance**: [Define accuracy requirements]

## Performance Requirements

### Timing

- **AGC execution time**: [microseconds/cycle]
- **Rust target**: [Match or better than AGC]
- **Maximum jitter**: [acceptable variation]

### Memory

- **AGC erasable usage**: [words]
- **AGC fixed usage**: [words]
- **Rust heap usage**: Should be zero (prefer stack allocation)

## Error Handling

### Abort Conditions

What should cause program abort?

| Condition | Detection | Recovery |
|-----------|-----------|----------|
| [Abort condition 1] | [How to detect] | [What happens] |
| [Abort condition 2] | [How to detect] | [What happens] |

### Failure Modes

How does the program handle failures?

| Failure | AGC Response | Rust Response |
|---------|--------------|---------------|
| Sensor failure | [AGC behavior] | [Rust behavior] |
| Convergence failure | [AGC behavior] | [Rust behavior] |
| Invalid state | [AGC behavior] | [Rust behavior] |

## Dependencies on Other Specs

Specs that must be implemented before this guidance program:

- [ ] [types-spec.md]: Core types (Vector3, Quaternion, Scaled, etc.)
- [ ] [imu-spec.md]: Inertial measurement unit interface
- [ ] [thrust-spec.md]: Thrust control interface
- [ ] [executive-spec.md]: Task scheduling

## Implementation Phases

### Phase 1: Skeleton
- [ ] Define trait and struct
- [ ] Implement basic initialization
- [ ] Stub out update cycle
- [ ] Unit test framework

### Phase 2: Core Algorithm
- [ ] Implement guidance equations
- [ ] Implement update cycle
- [ ] Implement termination logic
- [ ] Unit tests passing

### Phase 3: Integration
- [ ] Wire up external dependencies
- [ ] Integration tests passing
- [ ] Error handling complete

### Phase 4: Validation
- [ ] Historical validation test
- [ ] Performance benchmarks
- [ ] Code review

## Validation Criteria

Program is complete when:

- [ ] All unit tests passing
- [ ] Integration test completes full mission segment
- [ ] Historical validation within tolerance
- [ ] No clippy warnings
- [ ] Code review approved
- [ ] Documentation complete
- [ ] Performance requirements met

## Related Specs

- **Predecessor Program**: [P[XX-1]-spec.md]
- **Successor Program**: [P[XX+1]-spec.md]
- **Required Services**: [Links to service specs]

## Status

- [ ] Spec reviewed and approved
- [ ] Core algorithm implemented
- [ ] Tests written and passing
- [ ] Code reviewed
- [ ] Historical validation complete
- [ ] Integrated with executive

## References

- **AGC Source**: [GitHub links to exact files]
- **Technical Documentation**: [NASA reports, papers]
- **Mission Data**: [Apollo mission logs, journals]
- **Academic Papers**: [Relevant research papers]

## Notes

Additional context, questions, or observations:

- 
