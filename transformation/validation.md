# Validation

## Test Plan (Milestone 1 — EXECUTIVE + WAITLIST)

### Unit Tests (in `src/`)

| Test | Description | Status |
|------|-------------|--------|
| `highest_priority_runs_first` | Two jobs queued; lower priority must not run first | Not started |
| `all_slots_exhausted_triggers_alarm` | Fill 7 job slots; 8th call returns `NOVAC` | Not started |
| `job_completes_and_frees_slot` | Job finish restores free slot count | Not started |
| `waitlist_fires_after_delta_t` | Task deferred by N ticks fires on tick N | Not started |
| `waitlist_chain_fires_in_order` | Three chained waitlist entries fire in ascending delta order | Not started |
| `restart_resumes_at_saved_phase` | Simulated restart; job re-enters at last written phase index | Not started |
| `zero_priority_never_preempts` | Background-priority job does not run while higher-priority job is ready | Not started |

### Property Tests (via `proptest`)

| Property | Description | Status |
|----------|-------------|--------|
| Job slot invariant | After any sequence of start/end calls, `free_slots + active_slots == 7` | Not started |
| WAITLIST ordering | After any sequence of deferrals, the next-to-fire task always has the smallest remaining delta | Not started |

## Milestone 2 Validation (if navigation component added)

Compare component output against Virtual AGC emulator for an identical input state vector. Acceptable deviation: TBD based on interpreter floating-point scaling.

## Coverage Target

80% line coverage on `src/executive.rs` and `src/waitlist.rs` before Milestone 1 is declared complete.  
Tool: `cargo llvm-cov` or `cargo tarpaulin`.
