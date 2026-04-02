# AGC-to-Rust Transformation Tracking

This directory tracks the systematic transformation of Apollo Guidance Computer assembly code to idiomatic Rust.

## Tracking Files

| File | Purpose |
|------|---------|
| [progress.md](progress.md) | Overall transformation progress dashboard |
| [tasks.md](tasks.md) | Active tasks, priorities, and assignment tracking |
| [specifications.md](specifications.md) | Status of all specs (routine, module, guidance, interrupt, data) |
| [infrastructure.md](infrastructure.md) | Build system, tooling, dependencies, CI/CD setup |
| [validation.md](validation.md) | Test coverage, historical validation, benchmarks |
| [optimizations.md](optimizations.md) | Performance tracking and optimization notes |
| [decisions.md](decisions.md) | Architecture decision records (ADRs) |
| [blockers.md](blockers.md) | Current blockers and unresolved questions |

## Workflow Integration

### Creating a New Transformation

1. **Spec Phase**: Create spec in `../specs/` using appropriate template
2. **Track**: Add entry to [specifications.md](specifications.md) and [tasks.md](tasks.md)
3. **Implement**: Use `@Rust Implementer` with spec
4. **Validate**: Update [validation.md](validation.md) with test results
5. **Close**: Update [progress.md](progress.md) and mark task complete

### Status Meanings

- **Not Started**: Spec not yet created
- **Spec In Progress**: Writing/reviewing spec
- **Spec Complete**: Spec approved, ready for implementation
- **Implementation In Progress**: Code being written
- **Testing**: Tests being written or debugged
- **Review**: Code review in progress
- **Blocked**: Waiting on dependency or question resolution
- **Complete**: Implemented, tested, reviewed, integrated

## Quick Commands

```bash
# View overall progress
cat transformation/progress.md

# See active tasks
cat transformation/tasks.md | grep "In Progress"

# Check blockers
cat transformation/blockers.md

# Find incomplete specs
cat transformation/specifications.md | grep "Not Started\|In Progress"
```

## Related Documentation

- [specs/README.md](../specs/README.md) - Spec templates and workflow
- [Apollo.md](../Apollo.md) - AGC-to-Rust transformation guide
- [WORKSPACE.md](../WORKSPACE.md) - Development environment setup
- [AGENTS.md](../.github/AGENTS.md) - Rust coding conventions
