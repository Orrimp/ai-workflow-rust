# Blockers and Open Questions

## Active Blockers

_None currently_

## Open Questions

### Q1: Fixed-Point Library Choice

**Question**: Which fixed-point arithmetic library should we use?

**Options**:
1. `fixed` crate — mature, `no_std`-compatible
2. Custom `Scaled<N, T>` newtype with const generics — full control, compile-time scale checking

**Blocking**: Core types implementation

**Resolution**: Create ADR-003 after a brief spike comparing the two on AGC scaling factors.

---

### Q2: Workspace vs Single Crate

**Question**: Multi-crate workspace or single crate for MVP?

**Recommendation**: Start with a single crate (`agc-executive`). Split into a workspace only if the navigation component (Milestone 2) introduces genuine boundary needs.

**Blocking**: Initial `Cargo.toml` creation

**Resolution**: Create ADR-002.
