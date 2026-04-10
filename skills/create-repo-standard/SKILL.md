---
name: create-repo-standard
version: 1.0.0
categories:
  - scaffolding
  - productivity
  - developer-experience
triggers:
  - /create-repo-standard
  - scaffold repo
  - setup repository
  - init project standard
  - create repo standard
---

# Create Repo Standard

Interactive skill that scaffolds a production-ready repository with all the standard tooling, configuration, CI/CD, and AI agent instructions based on the selected language and ecosystem.

## Process

### Step 1: Gather Requirements (Interactive)

Ask the user the following questions **one group at a time**. Wait for answers before proceeding.

**Question 1 - Language:**
> What language/runtime is this project?
> 1. **TypeScript / JavaScript** (Node.js)
> 2. **Python**
> 3. **Go**
> 4. **Rust**

**Question 2 - Sub-selections (based on language):**

If **TypeScript / JavaScript**:
> Package manager?
> 1. **pnpm** (recommended)
> 2. **npm**
> 3. **yarn**
> 4. **bun**
>
> Framework (optional)?
> 1. None (library / CLI)
> 2. React (Vite)
> 3. Next.js
> 4. Express / Fastify
> 5. Cloudflare Workers

If **Python**:
> Package / env manager?
> 1. **uv** (recommended)
> 2. **poetry**
> 3. **pip + venv**
>
> Framework (optional)?
> 1. None (library / CLI)
> 2. FastAPI
> 3. Flask
> 4. Django

If **Go**:
> Framework (optional)?
> 1. None (standard library)
> 2. Chi
> 3. Gin
> 4. Echo

If **Rust**:
> Project type?
> 1. Library (lib)
> 2. Binary (bin)
> 3. Workspace

**Question 3 - Project metadata:**
> - Project name?
> - Brief description (one line)?
> - License? (default: MIT)

### Step 2: Initialize Project Foundation

Based on the language, run the appropriate init command:

- **TS/JS**: Initialize with the chosen package manager, create `tsconfig.json` (if TS), `package.json` with proper scripts
- **Python**: Initialize with `uv init`, `poetry init`, or create `pyproject.toml` manually
- **Go**: Run `go mod init <module-path>`
- **Rust**: Run `cargo init` with appropriate flags

Create the standard directory structure:

```
<project>/
├── src/              # Source code (or cmd/ + internal/ for Go)
├── tests/            # Test files (or *_test.go alongside for Go)
├── .github/
│   └── workflows/    # CI/CD workflows
├── .gitignore
├── Makefile
├── CLAUDE.md
├── AGENTS.md
├── LICENSE
└── README.md
```

### Step 3: Create .gitignore

Generate a comprehensive `.gitignore` tailored to the language:

**TypeScript / JavaScript:**
```
node_modules/
dist/
.turbo/
*.tsbuildinfo
coverage/
.env
.env.local
.env.*.local
.DS_Store
*.log
.wrangler/
.next/
.vscode/
!.vscode/settings.json
.idea/
```

**Python:**
```
__pycache__/
*.py[cod]
*$py.class
*.so
dist/
build/
*.egg-info/
.eggs/
.venv/
venv/
.env
.env.local
.mypy_cache/
.ruff_cache/
.pytest_cache/
htmlcov/
coverage.xml
*.log
.DS_Store
.vscode/
.idea/
```

**Go:**
```
bin/
dist/
vendor/
*.exe
*.exe~
*.dll
*.so
*.dylib
*.test
*.out
coverage.out
coverage.html
.env
.env.local
*.log
.DS_Store
.vscode/
.idea/
```

**Rust:**
```
/target
Cargo.lock  # Only for libraries, keep for binaries
*.log
.env
.env.local
.DS_Store
.vscode/
.idea/
```

### Step 4: Configure Formatter

Set up the language-appropriate formatter:

- **TS/JS**: Install and configure **Biome** (`biome.json`) with sensible defaults. Alternatively Prettier if the user prefers.
- **Python**: Configure **Ruff** formatter in `pyproject.toml` (`[tool.ruff.format]`)
- **Go**: Built-in `gofmt` / `goimports` - no config needed
- **Rust**: Create `rustfmt.toml` with standard settings

### Step 5: Configure Linter

Set up the language-appropriate linter:

- **TS/JS**: Configure **Biome** linting rules (or ESLint flat config if preferred). Enable recommended rules.
- **Python**: Configure **Ruff** linter in `pyproject.toml` (`[tool.ruff.lint]`) with recommended rule sets (`E`, `F`, `W`, `I`, `N`, `UP`, `B`, `SIM`)
- **Go**: Install and configure **golangci-lint** (`.golangci.yml`) with standard linters
- **Rust**: Configure Clippy settings in `Cargo.toml` or `clippy.toml`

### Step 6: Configure Testing

Set up the testing framework:

- **TS/JS**: Install and configure **Vitest** (`vitest.config.ts`) with coverage support
- **Python**: Configure **pytest** in `pyproject.toml` (`[tool.pytest.ini_options]`) with coverage via `pytest-cov`
- **Go**: Built-in `go test` - no extra setup needed, configure coverage flags in Makefile
- **Rust**: Built-in `cargo test` - add a basic test in `src/lib.rs` or a test file

Create a sample test file to verify the setup works.

### Step 7: Create Makefile

Create a `Makefile` with standard targets that wrap the language-specific commands:

```makefile
.PHONY: install build lint format test check clean

# Install dependencies
install:
	<language-specific install command>

# Build the project
build:
	<language-specific build command>

# Run linter
lint:
	<language-specific lint command>

# Run formatter
format:
	<language-specific format command>

# Run tests
test:
	<language-specific test command>

# Run tests with coverage
test-coverage:
	<language-specific test coverage command>

# Run all checks (format check + lint + type check + test)
check: format-check lint test

# Format check (CI mode - fail if not formatted)
format-check:
	<language-specific format check command>

# Clean build artifacts
clean:
	<language-specific clean command>
```

Actual commands per language:

**TS/JS (pnpm example):**
- install: `pnpm install`
- build: `pnpm run build`
- lint: `pnpm biome check --fix .`
- format: `pnpm biome format --write .`
- format-check: `pnpm biome format .`
- test: `pnpm vitest run`
- test-coverage: `pnpm vitest run --coverage`
- clean: `rm -rf dist node_modules .turbo`

**Python (uv example):**
- install: `uv sync`
- build: `uv build`
- lint: `uv run ruff check --fix .`
- format: `uv run ruff format .`
- format-check: `uv run ruff format --check .`
- test: `uv run pytest`
- test-coverage: `uv run pytest --cov --cov-report=html`
- clean: `rm -rf dist build *.egg-info .venv`

**Go:**
- install: `go mod download`
- build: `go build ./...`
- lint: `golangci-lint run`
- format: `gofmt -w .`
- format-check: `test -z "$$(gofmt -l .)"`
- test: `go test ./...`
- test-coverage: `go test -coverprofile=coverage.out ./... && go tool cover -html=coverage.out -o coverage.html`
- clean: `rm -rf bin dist coverage.out coverage.html`

**Rust:**
- install: (no-op or `cargo fetch`)
- build: `cargo build`
- lint: `cargo clippy -- -D warnings`
- format: `cargo fmt`
- format-check: `cargo fmt -- --check`
- test: `cargo test`
- test-coverage: `cargo tarpaulin --out html`
- clean: `cargo clean`

### Step 8: Create GitHub Actions Workflows

Create `.github/workflows/ci.yml`:

```yaml
name: CI

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  check:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      # Language-specific setup step (e.g., setup-node, setup-python, setup-go, rust-toolchain)

      - name: Install dependencies
        run: make install

      - name: Check formatting
        run: make format-check

      - name: Lint
        run: make lint

      - name: Build
        run: make build

      - name: Test
        run: make test
```

Include language-specific setup steps:

- **TS/JS**: `actions/setup-node@v4` + pnpm/npm/yarn/bun setup action
- **Python**: `actions/setup-python@v5` + uv/poetry install step
- **Go**: `actions/setup-go@v5`
- **Rust**: `dtolnay/rust-toolchain@stable` + `Swatinem/rust-cache@v2`

### Step 9: Create CLAUDE.md

Create a `CLAUDE.md` file with repo-specific instructions for Claude:

```markdown
# CLAUDE.md

## Project Overview

<project-name>: <description>

## Tech Stack

- **Language**: <language>
- **Package Manager**: <package-manager>
- **Formatter**: <formatter>
- **Linter**: <linter>
- **Test Framework**: <test-framework>
- **CI**: GitHub Actions

## Common Commands

All commands are available via Makefile:

- `make install` - Install dependencies
- `make build` - Build the project
- `make lint` - Run linter (with auto-fix)
- `make format` - Run formatter
- `make test` - Run tests
- `make check` - Run all checks (format + lint + test)
- `make clean` - Clean build artifacts

## Code Style

- Use <formatter> for formatting - do not manually format code
- Follow <linter> rules - run `make lint` before committing
- Write tests for all new functionality in the `tests/` directory
- Use conventional commits: `type(scope): description`

## Architecture

<Brief description of directory structure and where things go>

## Rules

- Never commit without running `make check` first
- Never skip pre-commit hooks
- Keep test coverage above the current level
- All PRs must pass CI before merging
```

### Step 10: Create AGENTS.md

Create an `AGENTS.md` file with instructions for AI agents working on this repo:

```markdown
# AGENTS.md

Instructions for AI agents (Claude, Copilot, etc.) working on this repository.

## Getting Started

1. Run `make install` to install all dependencies
2. Run `make check` to verify everything works
3. Read `CLAUDE.md` for project-specific conventions

## Development Workflow

1. Create a feature branch from `main`
2. Make changes following the code style guidelines
3. Run `make format` to auto-format code
4. Run `make lint` to check for issues (use auto-fix when available)
5. Run `make test` to ensure tests pass
6. Commit using conventional commits: `type(scope): description`

## Before Every Commit

Always run the full check suite:
```
make check
```

This runs formatting checks, linting, and tests. Do NOT commit if any check fails.

## Writing Tests

- Place tests in the `tests/` directory (or alongside source files for Go)
- Name test files with the `test_` prefix (Python), `*.test.ts` (TS/JS), `_test.go` (Go), or in `tests/` (Rust)
- Each new feature or bug fix MUST include corresponding tests
- Run `make test-coverage` to check coverage

## Code Review Checklist

Before submitting a PR, verify:
- [ ] All checks pass (`make check`)
- [ ] New code has tests
- [ ] No secrets or credentials in code
- [ ] Commit messages follow conventional commits
- [ ] PR description explains the "why"

## Troubleshooting

- If `make install` fails, check the language runtime version
- If linting fails with unfixable errors, investigate the root cause before disabling rules
- If tests are flaky, fix the flakiness rather than retrying
```

### Step 11: Create Supporting Files

1. **README.md** - Basic readme with project name, description, setup instructions, and available make commands
2. **LICENSE** - Generate the selected license (default MIT)
3. Initialize git if not already initialized (`git init`)

### Step 12: Install Dependencies and Verify

1. Run `make install` to install all dependencies
2. Run `make check` to verify the full pipeline works
3. If anything fails, fix it before finishing

### Step 13: Final Summary

Report to the user what was created:

```
Repository standard setup complete!

Created:
  - <package-config>     (project config)
  - <formatter-config>   (formatter)
  - <linter-config>      (linter)
  - Makefile             (build commands)
  - .gitignore           (ignore rules)
  - .github/workflows/   (CI pipeline)
  - CLAUDE.md            (AI agent instructions)
  - AGENTS.md            (AI agent workflow)
  - README.md            (documentation)
  - LICENSE              (MIT)
  - src/                 (source directory)
  - tests/               (test directory + sample test)

Available commands:
  make install       Install dependencies
  make build         Build the project
  make format        Format code
  make lint          Lint code
  make test          Run tests
  make check         Run all checks
```

## Rules

- Always ask questions interactively - never assume choices
- Use the most modern and recommended tooling for each language
- All generated configs should follow current best practices (as of 2025)
- The Makefile is the single entry point for all operations - all CI and commands go through it
- CLAUDE.md and AGENTS.md must be accurate and reflect the actual tools configured
- Every generated setup must pass `make check` before the skill is considered complete
- Do not over-engineer - keep configs minimal but complete
- Prefer tools that combine multiple functions (e.g., Biome for both format + lint in JS/TS, Ruff for both in Python)
