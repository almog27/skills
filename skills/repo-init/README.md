# repo-init

Interactive skill that scaffolds a production-ready repository with all the standard tooling, configuration, CI/CD, and AI agent instructions.

## Install

```bash
spm install repo-init
```

Or install SPM first if you don't have it:

```bash
# macOS
brew install skillpkg/tap/spm

# Linux / macOS
curl -fsSL https://skillpkg.dev/install.sh | sh

# npm
npm install -g @skillpkg/cli
```

## Usage

```
/repo-init
```

The skill will interactively ask you to choose:

1. **Language** - TypeScript/JavaScript, Python, Go, or Rust
2. **Sub-selections** - package manager (pnpm/npm/yarn/bun), framework, project type
3. **Project metadata** - name, description, license

## What it generates

| File | Purpose |
|------|---------|
| `Makefile` | Unified entry point: `install`, `build`, `lint`, `format`, `test`, `check`, `clean` |
| `.gitignore` | Language-specific ignore rules |
| Formatter config | Biome (JS/TS), Ruff (Python), gofmt (Go), rustfmt (Rust) |
| Linter config | Biome (JS/TS), Ruff (Python), golangci-lint (Go), Clippy (Rust) |
| Test setup | Vitest (JS/TS), pytest (Python), go test (Go), cargo test (Rust) |
| `.github/workflows/ci.yml` | CI pipeline running all checks via Makefile |
| `CLAUDE.md` | Claude Code agent instructions |
| `AGENTS.md` | Shared instructions for all AI agents |
| `.cursorrules` | Cursor IDE agent instructions |
| `.github/copilot-instructions.md` | GitHub Copilot instructions |
| `.windsurfrules` | Windsurf IDE agent instructions |
| `skills-lock.json` | SPM lock file (committed to git) |
| `README.md` | Project documentation |
| `LICENSE` | MIT (default) |

## Skills integration

Installs the [ship](https://skillpkg.dev/skills/ship) skill via SPM, which provides `/ship` - a full quality pipeline that formats, lints, type checks, builds, tests, commits, and pushes in one command.

## Makefile commands

Every generated repo uses the same Makefile interface regardless of language:

```bash
make install       # Install dependencies
make build         # Build the project
make format        # Format code
make lint          # Lint code
make test          # Run tests
make check         # Run all checks (format + lint + test)
make clean         # Clean build artifacts
```

## Supported languages

| Language | Package Manager | Formatter | Linter | Tests |
|----------|----------------|-----------|--------|-------|
| TypeScript/JS | pnpm, npm, yarn, bun | Biome | Biome | Vitest |
| Python | uv, poetry, pip+venv | Ruff | Ruff | pytest |
| Go | go modules | gofmt | golangci-lint | go test |
| Rust | cargo | rustfmt | Clippy | cargo test |

## License

MIT
