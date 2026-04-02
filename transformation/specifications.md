# Specification Tracking

Track the status of all transformation specs.

## Data Structures

| Spec | AGC Source | Status | Assignee | Notes |
|------|------------|--------|----------|-------|
| _No data structure specs yet_ | | | | |

**Summary**: 0 total, 0 complete (0%)

## Routines

| Spec | AGC Source | Status | Assignee | Notes |
|------|------------|--------|----------|-------|
| _No routine specs yet_ | | | | |

**Summary**: 0 total, 0 complete (0%)

## Modules

| Spec | AGC Source | Status | Assignee | Notes |
|------|------------|--------|----------|-------|
| _No module specs yet_ | | | | |

**Summary**: 0 total, 0 complete (0%)

## Guidance Algorithms

| Spec | AGC Source | Program # | Status | Assignee | Notes |
|------|------------|-----------|--------|----------|-------|
| _No guidance specs yet_ | | | | | |

**Summary**: 0 total, 0 complete (0%)

## Interrupt Handlers

| Spec | AGC Source | Priority | Status | Assignee | Notes |
|------|------------|----------|--------|----------|-------|
| _No interrupt specs yet_ | | | | | |

**Summary**: 0 total, 0 complete (0%)

## Status Legend

- **Not Started**: Spec file not yet created
- **Spec Draft**: Spec being written
- **Spec Review**: Spec under review
- **Spec Approved**: Ready for implementation
- **Implementation**: Code being written
- **Testing**: Tests being written/debugged
- **Code Review**: Implementation under review
- **Integration**: Being integrated with other components
- **Complete**: Fully implemented, tested, reviewed, integrated

## Spec Template Usage

When creating a new spec:

1. Copy appropriate template from `../specs/`
2. Fill out the spec following the template structure
3. Add entry to this tracking file
4. Link spec in [tasks.md](tasks.md)
5. Update status as work progresses

## Example Entry

```markdown
| [P63 Braking](../specs/p63-braking-spec.md) | Luminary099/P63.agc | P63 | Spec Approved | @Rust Implementer | Ready for impl |
```

## Validation Checklist

Before marking a spec "Spec Approved":

- [ ] AGC source referenced with file and line numbers
- [ ] All erasable variables documented with addresses and scaling
- [ ] Rust API designed with clear signatures
- [ ] At least 3 test cases with expected values
- [ ] Invariants explicitly stated
- [ ] Edge cases and error handling specified
- [ ] Dependencies identified
- [ ] Status checklist included

Before marking "Complete":

- [ ] Implementation matches spec
- [ ] All spec test cases implemented and passing
- [ ] Clippy clean
- [ ] Code reviewed
- [ ] Integration complete
- [ ] Spec status checkboxes marked
- [ ] Documentation complete

## Notes

- Specs in `../specs/` directory
- Use templates: routine-spec.md, module-spec.md, guidance-algorithm-spec.md, interrupt-spec.md, data-structure-spec.md
- Reference [specs/README.md](../specs/README.md) for workflow guidance
