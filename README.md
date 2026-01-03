# Reusable GitHub Actions Workflows

This repository contains reusable GitHub Actions workflows for Python, Node.js, and Go projects.

## Available Workflows

| Workflow | Description |
|----------|-------------|
| `python-ci.yml` | Python CI with uv, ruff linting, type checking, pytest |
| `python-security.yml` | Python security scanning (bandit, safety, semgrep, trivy, CodeQL) |
| `node-ci.yml` | Node.js CI with npm, linting, testing |
| `node-security.yml` | Node.js security scanning (npm audit, CodeQL, trivy) |
| `go-ci.yml` | Go CI with golangci-lint, testing, cross-platform builds |
| `claude.yml` | Claude PR assistant for @claude mentions |
| `claude-security-review.yml` | Claude-powered security code review |

## Usage

### Python Project

Create `.github/workflows/ci.yml`:

```yaml
name: CI

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  ci:
    uses: YOUR_USERNAME/workflows/.github/workflows/python-ci.yml@main
    with:
      package-name: your_package
      python-versions: '["3.11", "3.12", "3.13"]'
    secrets:
      CODECOV_TOKEN: ${{ secrets.CODECOV_TOKEN }}
```

Create `.github/workflows/security.yml`:

```yaml
name: Security

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]
  schedule:
    - cron: '0 2 * * 1'  # Weekly on Monday
  workflow_dispatch:

jobs:
  security:
    uses: YOUR_USERNAME/workflows/.github/workflows/python-security.yml@main
    with:
      package-name: your_package
```

### Node.js Project

Create `.github/workflows/ci.yml`:

```yaml
name: CI

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  ci:
    uses: YOUR_USERNAME/workflows/.github/workflows/node-ci.yml@main
    with:
      node-version: '22'
      run-build: true
```

### Go Project

Create `.github/workflows/ci.yml`:

```yaml
name: CI

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  ci:
    uses: YOUR_USERNAME/workflows/.github/workflows/go-ci.yml@main
    with:
      go-versions: '["1.21", "1.22"]'
      run-build: true
      binary-name: myapp
      binary-path: ./cmd/myapp
```

### Claude PR Assistant

Create `.github/workflows/claude.yml`:

```yaml
name: Claude

on:
  issue_comment:
    types: [created]
  pull_request_review_comment:
    types: [created]
  issues:
    types: [opened, assigned]
  pull_request_review:
    types: [submitted]

jobs:
  claude:
    uses: YOUR_USERNAME/workflows/.github/workflows/claude.yml@main
    secrets:
      CLAUDE_CODE_OAUTH_TOKEN: ${{ secrets.CLAUDE_CODE_OAUTH_TOKEN }}
```

## Workflow Inputs

### python-ci.yml

| Input | Type | Default | Description |
|-------|------|---------|-------------|
| `python-versions` | string | `'["3.11", "3.12", "3.13"]'` | JSON array of Python versions |
| `package-name` | string | **required** | Name of the Python package |
| `run-type-check` | boolean | `true` | Run type checking |
| `type-checker` | string | `'pyright'` | Type checker (`pyright` or `mypy`) |
| `run-lint` | boolean | `true` | Run ruff linting |
| `run-tests` | boolean | `true` | Run pytest |
| `test-path` | string | `'tests/'` | Path to test directory |

### python-security.yml

| Input | Type | Default | Description |
|-------|------|---------|-------------|
| `python-version` | string | `'3.11'` | Python version for scanning |
| `package-name` | string | **required** | Package name to scan |
| `run-bandit` | boolean | `true` | Run Bandit |
| `run-safety` | boolean | `true` | Run Safety |
| `run-semgrep` | boolean | `true` | Run Semgrep |
| `run-trivy` | boolean | `true` | Run Trivy |
| `run-codeql` | boolean | `true` | Run CodeQL |

### node-ci.yml

| Input | Type | Default | Description |
|-------|------|---------|-------------|
| `node-version` | string | `'22'` | Node.js version |
| `node-versions` | string | `''` | JSON array of Node versions (overrides node-version) |
| `run-lint` | boolean | `true` | Run linting |
| `run-tests` | boolean | `true` | Run tests |
| `run-build` | boolean | `false` | Run build |

### go-ci.yml

| Input | Type | Default | Description |
|-------|------|---------|-------------|
| `go-version` | string | `'1.21'` | Go version |
| `go-versions` | string | `''` | JSON array of Go versions |
| `run-lint` | boolean | `true` | Run golangci-lint |
| `run-tests` | boolean | `true` | Run tests |
| `run-build` | boolean | `false` | Build for multiple platforms |
| `binary-name` | string | `''` | Binary name for builds |
| `binary-path` | string | `'./cmd/...'` | Path to main package |

## Requirements

### Python Projects

- Use `pyproject.toml` with `uv` for dependency management
- Include dev dependencies for testing and linting:
  - `pytest`, `pytest-cov`
  - `ruff`
  - `pyright` or `mypy`

### Node.js Projects

- Use `package.json` with npm
- Include scripts for `lint` and `test`

### Go Projects

- Use Go modules (`go.mod`)
