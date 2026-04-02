# Task Tracking

## Active Tasks

_No active tasks yet_

## Backlog

### High Priority

- [ ] **Setup**: Create Rust workspace structure (Cargo.toml, src/, tests/)
- [ ] **Spec**: Define core fixed-point arithmetic types (data-structure-spec.md)
- [ ] **Spec**: Define Vector3/Quaternion types (data-structure-spec.md)
- [ ] **Infrastructure**: Set up CI/CD pipeline
- [ ] **Documentation**: Document project structure in README.md

### Medium Priority

- [ ] **Spec**: AGC memory model (erasable/fixed/channels)
- [ ] **Spec**: Basic I/O channel abstractions
- [ ] **Research**: Analyze existing Rust AGC implementations (ragc, apollo-gateway-rs)
- [ ] **Tooling**: Set up cargo-watch, cargo-nextest
- [ ] **Testing**: Define test oracle strategy

### Low Priority

- [ ] **Documentation**: Write contribution guide
- [ ] **Tooling**: Set up benchmarking framework
- [ ] **Research**: Investigate WASM compilation for web demos

## Completed

_No completed tasks yet_

## Task Template

When adding a new task, use this format:

```markdown
### [TASK-XXX] Task Title

**Type**: Spec / Implementation / Testing / Documentation / Infrastructure  
**Priority**: High / Medium / Low  
**Component**: [Which AGC component or Rust module]  
**Status**: Not Started / In Progress / Blocked / Complete  
**Assignee**: [Name or @Agent]  
**Estimate**: [Time estimate]  
**Dependencies**: [List of blocking tasks]

**Description**:
[Detailed description of what needs to be done]

**Acceptance Criteria**:
- [ ] Criterion 1
- [ ] Criterion 2
- [ ] Criterion 3

**Notes**:
[Any additional context, links, or observations]

**Related**:
- Spec: [Link to spec file if applicable]
- Issues: [Related blockers or issues]
```

## Kanban View

### Not Started

- Setup: Create Rust workspace structure
- All specs

### In Progress

_No tasks in progress_

### Blocked

_No blocked tasks_

### Review

_No tasks in review_

### Done

_No completed tasks_

## Sprint Planning

### Sprint 1 (TBD)

**Goal**: Establish foundation infrastructure

**Tasks**:
- [ ] Create Rust workspace
- [ ] Define core types spec
- [ ] Set up CI/CD
- [ ] First implementation (fixed-point arithmetic)

**Capacity**: TBD  
**Actual**: TBD

## Notes

- Use `@Rust Implementer` for implementation tasks
- Use `@Rust Code Review` before marking tasks complete
- Update [progress.md](progress.md) when tasks complete
- Link tasks to specs in [specifications.md](specifications.md)
