# Task Tracking

## Active Tasks

_None_

## Backlog

### Milestone 1 — EXECUTIVE + WAITLIST

- [ ] **ADR-002** — Decide workspace vs single crate; create `Cargo.toml`
- [ ] **ADR-003** — Spike fixed-point library (`fixed` crate vs custom newtype); document decision
- [ ] **Spec** — Write `EXECUTIVE` module spec (`specs/executive-module-spec.md`)
- [ ] **Impl** — Implement `Executive` scheduler (job table, priority ordering, `STARTJOB`/`ENDJOB`/`NOVAC`)
- [ ] **Spec** — Write `WAITLIST` module spec (`specs/waitlist-module-spec.md`)
- [ ] **Impl** — Implement `Waitlist` (delta-time chain, up to 9 pending tasks)
- [ ] **Impl** — Phase-table restart protection
- [ ] **Tests** — All scheduler unit tests passing

### Milestone 2 — Navigation Component (optional)

- [ ] Choose nav component (`SERVICER`, `ORBITAL_INTEGRATION`, or `CONIC_SUBROUTINES`)
- [ ] Write spec file
- [ ] Implement and schedule via EXECUTIVE
- [ ] Validate against Virtual AGC

## Completed

_No completed tasks yet_
