# Vibe Coding Experience Setup

This guide helps you set up an optimal Rust development experience with fast feedback loops, powerful tooling, and smooth workflows.

## ✅ Already Configured

- **AI Skills & Agents**: Custom rust-development, rust-testing, rust-debugging skills + specialized agents
- **Git setup**: Repository initialized and ready
- **Customization framework**: `.github/` structure with skills, agents, and instructions

## 🎯 Essential Tools

### Fast Feedback Loop

Install cargo-watch for automatic recompilation on file changes:

```powershell
cargo install cargo-watch
```

Usage while coding:
```powershell
# Watch and run checks + tests
cargo watch -x check -x test

# Watch and run your binary
cargo watch -x check -x test -x run

# Clear screen between runs
cargo watch -c -x check -x test
```

### Recommended VS Code Extensions

1. **rust-analyzer** - Instant errors, autocomplete, inline type hints
2. **Error Lens** - Inline error messages (no hovering needed)
3. **CodeLLDB** or **rust-debugger** - Visual debugging with breakpoints
4. **Even Better TOML** - Cargo.toml syntax highlighting
5. **GitLens** - Inline git blame and history navigation

### Terminal Enhancement Tools

Modern alternatives for smoother command-line workflows:

```powershell
cargo install bat       # Better 'cat' with syntax highlighting
cargo install exa       # Better 'ls' with git status integration
cargo install ripgrep   # Blazing fast search (rg command)
cargo install fd-find   # Better 'find' command (fd command)
```

### Quality-of-Life Cargo Tools

```powershell
cargo install cargo-expand    # See macro expansions
cargo install cargo-audit     # Security vulnerability checks
cargo install cargo-outdated  # Check for dependency updates
cargo install cargo-nextest   # Faster, better test runner
cargo install cargo-edit      # cargo add/rm/upgrade commands
```

## 📁 VS Code Configuration

### .vscode/settings.json

Create `.vscode/settings.json` with:

```json
{
  "rust-analyzer.checkOnSave.command": "clippy",
  "rust-analyzer.inlayHints.enable": true,
  "rust-analyzer.inlayHints.chainingHints.enable": true,
  "rust-analyzer.inlayHints.parameterHints.enable": true,
  "editor.formatOnSave": true,
  "editor.inlayHints.enabled": "on",
  "editor.semanticHighlighting.enabled": true,
  "files.watcherExclude": {
    "**/target/**": true
  },
  "[rust]": {
    "editor.defaultFormatter": "rust-lang.rust-analyzer",
    "editor.rulers": [100]
  }
}
```

### .vscode/tasks.json

Create `.vscode/tasks.json` for quick commands:

```json
{
  "version": "2.0.0",
  "tasks": [
    {
      "label": "cargo watch",
      "type": "shell",
      "command": "cargo watch -x check -x test",
      "isBackground": true,
      "problemMatcher": "$rustc",
      "presentation": {
        "reveal": "always",
        "panel": "new"
      }
    },
    {
      "label": "cargo check",
      "type": "shell",
      "command": "cargo check",
      "problemMatcher": "$rustc",
      "group": {
        "kind": "build",
        "isDefault": true
      }
    },
    {
      "label": "cargo test",
      "type": "shell",
      "command": "cargo test",
      "problemMatcher": "$rustc",
      "group": {
        "kind": "test",
        "isDefault": true
      }
    },
    {
      "label": "cargo clippy",
      "type": "shell",
      "command": "cargo clippy -- -D warnings",
      "problemMatcher": "$rustc"
    }
  ]
}
```

### .vscode/launch.json

Create `.vscode/launch.json` for debugging:

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "type": "lldb",
      "request": "launch",
      "name": "Debug executable",
      "cargo": {
        "args": [
          "build",
          "--bin=${workspaceFolderBasename}",
          "--package=${workspaceFolderBasename}"
        ],
        "filter": {
          "name": "${workspaceFolderBasename}",
          "kind": "bin"
        }
      },
      "args": [],
      "cwd": "${workspaceFolder}"
    },
    {
      "type": "lldb",
      "request": "launch",
      "name": "Debug unit tests",
      "cargo": {
        "args": [
          "test",
          "--no-run",
          "--lib"
        ]
      },
      "args": [],
      "cwd": "${workspaceFolder}"
    }
  ]
}
```

## ⌨️ Keyboard Shortcuts

With the above setup:

- **Ctrl+Shift+B** → Run default build task (cargo check)
- **Ctrl+Shift+T** → Run tests
- **F5** → Start debugging
- **Ctrl+Shift+P** → Command palette → "Tasks: Run Task" → Select custom tasks

## 🎨 Optional Vibe Enhancers

### Editor Appearance

- **Themes**: Monokai Pro, Nord, Dracula, One Dark Pro, Tokyo Night
- **Fonts with ligatures**: JetBrains Mono, Fira Code, Cascadia Code, Victor Mono
- **Icon themes**: vscode-icons, Material Icon Theme

### Background Enhancements

- **Music**: Lofi beats, Brain.fm, or ambient sounds for focus
- **GitHub Copilot**: Enable inline suggestions (not just chat)
- **Zen Mode**: Ctrl+K Z for distraction-free coding

### Advanced Workflow Tools

```powershell
# Task runner with Makefile.toml
cargo install cargo-make

# Generate flamegraphs for profiling
cargo install flamegraph

# Better benchmarking
cargo install cargo-criterion

# Dependency tree visualization
cargo install cargo-tree
```

## 🚀 Quick Start Workflow

### Initial Setup

1. Install rust-analyzer extension in VS Code (if not already installed)
2. Install cargo-watch: `cargo install cargo-watch`
3. Create `.vscode/` directory with settings.json, tasks.json, launch.json
4. Install recommended extensions

### Daily Coding Flow

1. **Open terminal** and start watch mode:
   ```powershell
   cargo watch -x check -x test
   ```

2. **Code with confidence**: 
   - Instant feedback from rust-analyzer
   - Auto-recompilation on save via cargo-watch
   - AI assistance on demand via `/rust-development`, `/rust-testing`, etc.

3. **Use custom agents when needed**:
   - Complex implementation → `@Rust Implementer`
   - Debugging issues → `@Rust Debugger`
   - Code review → `@Rust Code Review`

4. **Before committing**:
   ```powershell
   cargo fmt
   cargo clippy -- -D warnings
   cargo test
   ```

## 🔧 Embedded Development Additions

If working on no_std/bare-metal projects:

```powershell
# Install targets
rustup target add thumbv7em-none-eabihf  # Example: ARM Cortex-M4
rustup target add riscv32imac-unknown-none-elf  # Example: RISC-V

# Tools for embedded
cargo install cargo-binutils
cargo install probe-run
cargo install cargo-embed
cargo install cargo-flash
rustup component add llvm-tools-preview
```

Update `.vscode/settings.json`:
```json
{
  "rust-analyzer.check.allTargets": false,
  "rust-analyzer.checkOnSave.extraArgs": ["--target", "thumbv7em-none-eabihf"]
}
```

## 📚 Additional Resources

- **Rust Skills**: Located in `.github/skills/` - loaded automatically when relevant
- **Custom Agents**: Defined in `.github/agents/` - invoke with `@Agent Name`
- **Workspace Instructions**: See `AGENTS.md` for project-wide conventions
- **Apollo AGC Guide**: See `Apollo.md` for AGC-to-Rust transformation guidance

## 🎯 Goals

The vibe coding experience prioritizes:

- **Fast feedback**: See errors immediately, test on save
- **Minimal friction**: Automation handles formatting, checking, testing
- **Clear insights**: Inline hints, error messages, and suggestions
- **AI-assisted**: Skills and agents provide domain expertise on demand
- **Focused flow**: Clean UI, good music, distraction-free environment

Happy coding! 🦀
