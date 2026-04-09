# Workspace Setup

## Essential Cargo Tools

```sh
cargo install cargo-watch      # auto-recompile on save
cargo install cargo-nextest    # faster test runner
cargo install cargo-audit      # security advisories
cargo install cargo-outdated   # stale dependency check
cargo install cargo-expand     # macro expansion debug
```

## Daily Commands

```sh
cargo watch -x check -x test   # continuous feedback
cargo fmt && cargo clippy -- -D warnings && cargo test  # pre-commit
cargo nextest run               # fast test run
cargo audit                     # security check
```

## VS Code

Enable `rust-analyzer.checkOnSave.command = "clippy"` and `editor.formatOnSave = true`.

See `AGENTS.md` for project conventions and `Apollo.md` for AGC architecture reference.
