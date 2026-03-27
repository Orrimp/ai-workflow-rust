# Rust Project Guidelines

## Code Style
- Prefer stable Rust unless the repository explicitly opts into nightly features.
- Follow existing module and naming patterns before introducing new abstractions.
- Use meaningful, descriptive names and standard Rust naming conventions: `snake_case` for functions, variables, and modules; `PascalCase` for types and traits; `SCREAMING_SNAKE_CASE` for constants.
- Keep public APIs small and explicit. Prefer structs, enums, and traits with clear ownership semantics over hidden side effects.
- Prefer simple API surfaces: use `&T`, `&mut T`, or owned values in public APIs instead of exposing `Arc<T>`, `Rc<T>`, `Box<T>`, `RefCell<T>`, or similar wrappers unless the wrapper is fundamental to the API.
- Prefer concrete types first, then generics, and only then `dyn Trait` where the flexibility is required by callers.
- Avoid `unwrap` and `expect` in production code unless a failure is provably unrecoverable and documented by the surrounding code.
- Use `Result`-based error handling for fallible paths. Prefer standard library types first and only add error crates when the repository already uses them or there is a clear payoff.
- Prefer standard library types in public APIs over third-party crate types unless leaking the external type has a clear interoperability benefit.
- Avoid redundant comments. Add comments for Rust-specific nuance, invariants, safety, or non-obvious tradeoffs, not for code that is already clear from the implementation.

## Architecture
- Keep domain logic separate from transport, CLI, or persistence layers.
- Prefer composition over macro-heavy or deeply generic designs unless the project already uses those patterns.
- Minimize shared mutable state. Favor borrowing, immutable data, and narrow mutation scopes.
- Keep I/O mockable and avoid ad-hoc filesystem, network, or time access deep inside domain logic. Pass I/O capabilities in explicitly where practical.
- Accept ergonomic input types for functions when it helps callers, such as `impl AsRef<Path>`, `impl AsRef<str>`, or `impl Read`/`impl Write` for one-shot I/O.
- If responsibilities diverge enough to improve maintenance, testability, or compile times, split the crate or workspace instead of growing a catch-all module tree.
- Add dependencies cautiously. Prefer the standard library or existing crates already present in the workspace.
- Make fields private by default and expose accessors or builders when that improves invariants and API clarity.

## Build And Test
- Validate Rust changes with `cargo fmt`, `cargo clippy`, and `cargo test` when the project structure supports them.
- Use static verification aggressively. When appropriate, also consider `cargo audit`, `cargo udeps`, `cargo hack`, and `miri` for unsafe code or feature-heavy crates.
- Run the narrowest useful command first, then broaden validation if the change affects shared code.
- Add unit tests close to the code they verify. Use integration tests in `tests/` for externally visible behavior.
- Keep test-only helpers behind clearly named test features such as `test-util` when they would weaken production invariants.
- Keep feature flags additive and easy to reason about. Avoid features that remove behavior or create surprising downstream combinations.
- Keep the tree clean: do not leave commented-out code, stray `dbg!`, or temporary `println!` debugging in finished changes.

## Conventions
- Document public modules, types, and functions when their behavior is not obvious from the signature. Summary sentences should stay short and examples should be directly usable.
- Public items should document error, panic, and safety behavior when applicable.
- Public types should usually implement `Debug`; types intended for human-readable output should also implement `Display`.
- Keep functions focused. Split large functions when they mix parsing, transformation, and I/O.
- Prefer borrowing over ownership when it keeps APIs clear and avoids unnecessary allocation.
- Keep function parameter lists short; when a function needs many configuration values, prefer a dedicated config or builder type.
- Prefer regular methods over associated functions when a receiver makes the API clearer.
- Prefer dedicated error structs with contextual information over one global catch-all error enum. Panics are for programming bugs or unrecoverable stop-the-program conditions, not ordinary control flow.
- Detecting a programming bug should usually result in a panic, not a recoverable error.
- Avoid wildcard imports except where Rust convention makes them appropriate, such as `use super::*` in tests or explicit preludes.
- Prefer `#[expect(..., reason = "...")]` over local `#[allow(...)]` when suppressing lints in handwritten code.
- Any use of `unsafe` must be justified in plain language and kept narrow.
- Document non-obvious constants and magic values, including why they were chosen.
- For async code, stay consistent with the runtime already in use. Do not introduce a new runtime casually.
- For async CPU-heavy or long-running work, add cooperative yield points when the code would otherwise monopolize the executor.
- For async applications, prefer `tokio` if the repository is choosing a runtime from scratch; for CPU-bound parallelism, prefer dedicated worker threads or `rayon` instead of blocking the async executor.
- Use `cargo` as the default interface for build, test, lint, docs, and dependency workflows.
- Preserve formatting and organization in unrelated files. Do not rewrite large sections without a task-specific reason.