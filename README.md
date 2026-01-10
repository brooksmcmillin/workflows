# Reusable GitHub Actions Workflows

This repository contains reusable GitHub Actions workflows for Python, Node.js, Go, Rust, and Hugo projects.

## Available Workflows

| Workflow | Description |
|----------|-------------|
| `python-ci.yml` | Python CI with uv, ruff linting, type checking, pytest, build verification |
| `python-precommit.yml` | Fast pre-commit checks (ruff, pyright, pytest, bandit) for PR validation |
| `python-security.yml` | Python security scanning (bandit, safety, semgrep, trivy, CodeQL) |
| `python-release.yml` | Automated PyPI publishing with multi-OS install testing |
| `node-ci.yml` | Node.js CI with npm, linting, testing |
| `node-security.yml` | Node.js security scanning (npm audit, CodeQL, trivy) |
| `go-ci.yml` | Go CI with golangci-lint, testing, cross-platform builds |
| `rust-ci.yml` | Rust CI with clippy, rustfmt, testing with coverage, cross-platform builds |
| `rust-precommit.yml` | Fast pre-commit checks (clippy, rustfmt, test, cargo-audit) for PR validation |
| `rust-security.yml` | Rust security scanning (cargo-audit, cargo-deny, semgrep, trivy) |
| `ansible-ci.yml` | Ansible CI with yamllint, ansible-lint, syntax checking |
| `hugo-build.yml` | Hugo site build verification |
| `hugo-accessibility.yml` | Pa11y-ci accessibility testing for Hugo sites |
| `hugo-spellcheck.yml` | CSpell spelling checks for Hugo content |
| `hugo-security-headers.yml` | Security header validation with Mozilla Observatory |
| `hugo-links.yml` | Lychee link checking for Hugo sites |
| `hugo-html-validation.yml` | HTML5 validation for Hugo output |
| `hugo-lighthouse.yml` | Lighthouse CI performance and quality testing |
| `hugo-feed-validation.yml` | RSS/XML feed validation for Hugo sites |
| `claude.yml` | Claude PR assistant for @claude mentions |
| `claude-security-review.yml` | Claude-powered security code review |

## Configuration Files

| File | Description |
|------|-------------|
| `examples/dependabot.yml` | Dependabot config for automated dependency updates |

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

Create `.github/workflows/precommit.yml` (lightweight alternative to full CI):

```yaml
name: Pre-commit

on:
  pull_request:
    branches: [main]

jobs:
  precommit:
    uses: YOUR_USERNAME/workflows/.github/workflows/python-precommit.yml@main
    with:
      package-name: your_package
      # Optional: customize checks
      # python-version: '3.12'
      # run-security: false  # Disable bandit if using python-security.yml separately
    secrets:
      CODECOV_TOKEN: ${{ secrets.CODECOV_TOKEN }}
```

Create `.github/workflows/release.yml`:

```yaml
name: Release

on:
  push:
    tags:
      - 'v*'
  workflow_dispatch:

jobs:
  release:
    uses: YOUR_USERNAME/workflows/.github/workflows/python-release.yml@main
    with:
      publish-to-pypi: true
    secrets:
      PYPI_API_TOKEN: ${{ secrets.PYPI_API_TOKEN }}
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

### Rust Project

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
    uses: YOUR_USERNAME/workflows/.github/workflows/rust-ci.yml@main
    with:
      rust-versions: '["stable", "beta"]'
      run-build: true
      binary-name: myapp
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
    uses: YOUR_USERNAME/workflows/.github/workflows/rust-security.yml@main
```

Create `.github/workflows/precommit.yml` (lightweight alternative to full CI):

```yaml
name: Pre-commit

on:
  pull_request:
    branches: [main]

jobs:
  precommit:
    uses: YOUR_USERNAME/workflows/.github/workflows/rust-precommit.yml@main
    with:
      rust-version: 'stable'
      # Optional: customize checks
      # run-security: false  # Disable cargo-audit if using rust-security.yml separately
    secrets:
      CODECOV_TOKEN: ${{ secrets.CODECOV_TOKEN }}
```

### Ansible Project

Create `.github/workflows/ci.yml`:

```yaml
name: CI

on:
  push:
    branches: [main, master]
  pull_request:
    branches: [main, master]

jobs:
  ci:
    uses: YOUR_USERNAME/workflows/.github/workflows/ansible-ci.yml@main
    with:
      ansible-version: 'latest'
      python-version: '3.12'
      # Optional: specify playbooks to check (auto-detects by default)
      # playbook-paths: 'site.yml deploy.yml'
```

### Hugo Project

For a Hugo static site, create the following workflows:

**Build** (`.github/workflows/build.yml`):

```yaml
name: Build

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  build:
    uses: YOUR_USERNAME/workflows/.github/workflows/hugo-build.yml@main
    with:
      hugo-version: '0.128.0'
```

**Accessibility** (`.github/workflows/accessibility.yml`):

```yaml
name: Accessibility Check

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]
  workflow_dispatch:

jobs:
  accessibility:
    uses: YOUR_USERNAME/workflows/.github/workflows/hugo-accessibility.yml@main
    with:
      hugo-version: '0.128.0'
      urls: '/ /about/ /blog/'
```

**Spell Check** (`.github/workflows/spellcheck.yml`):

```yaml
name: Spell Check

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  spellcheck:
    uses: YOUR_USERNAME/workflows/.github/workflows/hugo-spellcheck.yml@main
```

**Link Check** (`.github/workflows/links.yml`):

```yaml
name: Check Links

on:
  schedule:
    - cron: '0 0 * * 0'  # Weekly
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  check-links:
    uses: YOUR_USERNAME/workflows/.github/workflows/hugo-links.yml@main
    with:
      hugo-version: '0.128.0'
      exclude-patterns: 'localhost,linkedin.com'
```

**HTML Validation** (`.github/workflows/html-validation.yml`):

```yaml
name: HTML Validation

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  validate:
    uses: YOUR_USERNAME/workflows/.github/workflows/hugo-html-validation.yml@main
    with:
      hugo-version: '0.128.0'
```

**Lighthouse CI** (`.github/workflows/lighthouse.yml`):

```yaml
name: Lighthouse CI

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  lighthouse:
    uses: YOUR_USERNAME/workflows/.github/workflows/hugo-lighthouse.yml@main
    with:
      hugo-version: '0.128.0'
      urls: '/ /about/ /blog/'
```

**Feed Validation** (`.github/workflows/feed-validation.yml`):

```yaml
name: Feed Validation

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  validate-feeds:
    uses: YOUR_USERNAME/workflows/.github/workflows/hugo-feed-validation.yml@main
    with:
      hugo-version: '0.128.0'
```

**Security Headers** (`.github/workflows/security-headers.yml`):

```yaml
name: Security Headers Check

on:
  schedule:
    - cron: '0 0 * * 1'  # Weekly on Monday
  workflow_dispatch:

jobs:
  security-headers:
    uses: YOUR_USERNAME/workflows/.github/workflows/hugo-security-headers.yml@main
    with:
      site-url: 'https://example.com'
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

### Dependabot

Copy `examples/dependabot.yml` to your project's `.github/dependabot.yml` and uncomment the relevant sections for your project type.

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
| `run-build` | boolean | `true` | Build and verify package |
| `test-path` | string | `'tests/'` | Path to test directory |

### python-precommit.yml

| Input | Type | Default | Description |
|-------|------|---------|-------------|
| `python-version` | string | `'3.12'` | Python version for checks |
| `package-name` | string | **required** | Name of the Python package |
| `test-path` | string | `'tests/'` | Path to test directory |
| `source-path` | string | `'src/'` | Path to source directory (for security scans) |
| `run-lint` | boolean | `true` | Run ruff linting |
| `run-format` | boolean | `true` | Run ruff format check |
| `run-type-check` | boolean | `true` | Run pyright type checking |
| `run-tests` | boolean | `true` | Run pytest with coverage |
| `run-security` | boolean | `true` | Run bandit security scan |
| `lint-command` | string | `''` | Custom lint command (overrides ruff) |
| `format-command` | string | `''` | Custom format command |
| `type-check-command` | string | `''` | Custom type check command |
| `test-command` | string | `''` | Custom test command |
| `fail-fast` | boolean | `true` | Fail immediately on first error |
| `upload-coverage` | boolean | `true` | Upload coverage to Codecov |

### python-release.yml

| Input | Type | Default | Description |
|-------|------|---------|-------------|
| `python-version` | string | `'3.11'` | Python version for building |
| `publish-to-pypi` | boolean | `true` | Publish to PyPI |
| `publish-to-testpypi` | boolean | `false` | Publish to TestPyPI first |
| `build-only` | boolean | `false` | Only build, don't publish |

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

### rust-ci.yml

| Input | Type | Default | Description |
|-------|------|---------|-------------|
| `rust-version` | string | `'stable'` | Rust toolchain version |
| `rust-versions` | string | `''` | JSON array of Rust versions (overrides rust-version) |
| `run-lint` | boolean | `true` | Run clippy linting |
| `run-format` | boolean | `true` | Run rustfmt format check |
| `run-tests` | boolean | `true` | Run cargo test |
| `run-build` | boolean | `false` | Build for multiple platforms |
| `clippy-args` | string | `'-- -D warnings'` | Additional arguments for clippy |
| `test-args` | string | `''` | Additional arguments for cargo test |
| `build-targets` | string | `'[{"target":"x86_64-unknown-linux-gnu"},...]'` | JSON array of build targets |
| `binary-name` | string | `''` | Binary name for builds |

### rust-precommit.yml

| Input | Type | Default | Description |
|-------|------|---------|-------------|
| `rust-version` | string | `'stable'` | Rust toolchain version |
| `run-lint` | boolean | `true` | Run clippy linting |
| `run-format` | boolean | `true` | Run rustfmt format check |
| `run-tests` | boolean | `true` | Run cargo test |
| `run-build` | boolean | `true` | Run cargo build |
| `run-security` | boolean | `true` | Run cargo-audit security scan |
| `run-doc` | boolean | `false` | Run cargo doc to check documentation |
| `clippy-args` | string | `'-- -D warnings'` | Additional arguments for clippy |
| `test-args` | string | `''` | Additional arguments for cargo test |
| `build-args` | string | `''` | Additional arguments for cargo build |
| `fail-fast` | boolean | `true` | Fail immediately on first error |
| `upload-coverage` | boolean | `true` | Upload coverage to Codecov |

### rust-security.yml

| Input | Type | Default | Description |
|-------|------|---------|-------------|
| `rust-version` | string | `'stable'` | Rust toolchain version |
| `run-cargo-audit` | boolean | `true` | Run cargo-audit vulnerability scanning |
| `run-cargo-deny` | boolean | `true` | Run cargo-deny license and advisory checks |
| `run-trivy` | boolean | `true` | Run Trivy filesystem scan |
| `run-codeql` | boolean | `false` | Run CodeQL analysis |
| `run-semgrep` | boolean | `true` | Run Semgrep security scan |
| `deny-config` | string | `''` | Path to deny.toml config file |

### ansible-ci.yml

| Input | Type | Default | Description |
|-------|------|---------|-------------|
| `ansible-version` | string | `'latest'` | Ansible version (`latest`, `2.16`, `2.17`, etc.) |
| `python-version` | string | `'3.12'` | Python version for running Ansible |
| `run-yamllint` | boolean | `true` | Run yamllint on YAML files |
| `run-ansible-lint` | boolean | `true` | Run ansible-lint |
| `run-syntax-check` | boolean | `true` | Run ansible-playbook --syntax-check |
| `playbook-paths` | string | `''` | Space-separated playbook paths (auto-detects if empty) |
| `lint-config` | string | `''` | Path to ansible-lint config file |
| `yamllint-config` | string | `''` | Path to yamllint config file |

### hugo-build.yml

| Input | Type | Default | Description |
|-------|------|---------|-------------|
| `hugo-version` | string | `'0.128.0'` | Hugo version to use |
| `extended` | boolean | `true` | Use Hugo extended version |
| `build-args` | string | `'--gc --minify'` | Additional arguments for hugo build |
| `submodules` | string | `'true'` | Checkout submodules (true, false, or recursive) |

### hugo-accessibility.yml

| Input | Type | Default | Description |
|-------|------|---------|-------------|
| `hugo-version` | string | `'0.128.0'` | Hugo version to use |
| `extended` | boolean | `true` | Use Hugo extended version |
| `build-args` | string | `'--gc --minify'` | Additional arguments for hugo build |
| `submodules` | string | `'true'` | Checkout submodules |
| `node-version` | string | `'20'` | Node.js version |
| `pa11y-config` | string | `'.pa11yci.json'` | Path to pa11y-ci config file |
| `urls` | string | `'/ /about/ /blog/'` | Space-separated list of URLs to test |
| `server-port` | string | `'8080'` | Port to run local server on |

### hugo-spellcheck.yml

| Input | Type | Default | Description |
|-------|------|---------|-------------|
| `files` | string | `'content/**/*.md\ncontent/**/*.html'` | File patterns to check (newline-separated) |
| `config` | string | `'.cspell.json'` | Path to cspell config file |
| `inline` | string | `'warning'` | Inline handling (error, warning, off) |
| `strict` | boolean | `false` | Strict mode - fail on any spelling issue |

### hugo-security-headers.yml

| Input | Type | Default | Description |
|-------|------|---------|-------------|
| `site-url` | string | **required** | URL of the site to check |
| `required-headers` | string | `'Strict-Transport-Security X-Content-Type-Options...'` | Space-separated list of required headers |
| `run-mozilla-observatory` | boolean | `true` | Run Mozilla Observatory scan |

### hugo-links.yml

| Input | Type | Default | Description |
|-------|------|---------|-------------|
| `hugo-version` | string | `'0.128.0'` | Hugo version to use |
| `extended` | boolean | `true` | Use Hugo extended version |
| `build-args` | string | `'--gc --minify'` | Additional arguments for hugo build |
| `submodules` | string | `'true'` | Checkout submodules |
| `lychee-args` | string | `'--verbose --no-progress...'` | Additional arguments for lychee |
| `exclude-patterns` | string | `'localhost,linkedin.com'` | URL patterns to exclude (comma-separated) |
| `fail-on-error` | boolean | `true` | Fail the workflow on broken links |
| `path` | string | `'./public/**/*.html'` | Path pattern to check |

### hugo-html-validation.yml

| Input | Type | Default | Description |
|-------|------|---------|-------------|
| `hugo-version` | string | `'0.128.0'` | Hugo version to use |
| `extended` | boolean | `true` | Use Hugo extended version |
| `build-args` | string | `'--gc --minify'` | Additional arguments for hugo build |
| `submodules` | string | `'true'` | Checkout submodules |
| `root` | string | `'public/'` | Root directory to validate |
| `css` | boolean | `false` | Validate CSS files |
| `blacklist` | string | `''` | Files to exclude from validation |

### hugo-lighthouse.yml

| Input | Type | Default | Description |
|-------|------|---------|-------------|
| `hugo-version` | string | `'0.128.0'` | Hugo version to use |
| `extended` | boolean | `true` | Use Hugo extended version |
| `build-args` | string | `'--gc --minify'` | Additional arguments for hugo build |
| `submodules` | string | `'true'` | Checkout submodules |
| `node-version` | string | `'20'` | Node.js version |
| `lhci-version` | string | `'0.14.x'` | Lighthouse CI version |
| `urls` | string | `'/ /about/ /blog/'` | Space-separated list of URLs to test |
| `performance-assertion` | string | `'off'` | Performance category assertion (off, warn, error) |
| `accessibility-assertion` | string | `'warn'` | Accessibility category assertion |
| `best-practices-assertion` | string | `'warn'` | Best practices category assertion |
| `seo-assertion` | string | `'warn'` | SEO category assertion |
| `upload-target` | string | `'temporary-public-storage'` | Upload target for results |

### hugo-feed-validation.yml

| Input | Type | Default | Description |
|-------|------|---------|-------------|
| `hugo-version` | string | `'0.128.0'` | Hugo version to use |
| `extended` | boolean | `true` | Use Hugo extended version |
| `build-args` | string | `'--gc --minify'` | Additional arguments for hugo build |
| `submodules` | string | `'true'` | Checkout submodules |
| `validate-sitemap` | boolean | `true` | Validate sitemap.xml |
| `validate-rss` | boolean | `true` | Validate RSS feeds |
| `check-rss-structure` | boolean | `true` | Check RSS feed structure for required elements |

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

### Rust Projects

- Use Cargo with `Cargo.toml` and `Cargo.lock`
- For cargo-deny: optional `deny.toml` config file
- For coverage: `cargo-llvm-cov` is installed automatically

### Ansible Projects

- Standard Ansible directory structure
- Playbooks with `hosts:` directive for auto-detection
- Optional: `requirements.yml` for collection dependencies

### Hugo Projects

- Hugo site with standard directory structure
- For accessibility testing: `.pa11yci.json` config file
- For spell checking: `.cspell.json` config file
- Optional: Git submodules for themes

## Future Improvements

The following enhancements are planned or could be added:

### Workflows

- [ ] **docker.yml** - Build, scan, and push Docker images to registries (GHCR, Docker Hub)
- [ ] **node-release.yml** - Automated npm publishing on tags
- [ ] **go-release.yml** - Automated Go release with goreleaser
- [ ] **pr-labeler.yml** - Auto-label PRs based on files changed
- [ ] **release-please.yml** - Automated versioning and changelogs using conventional commits
- [ ] **stale.yml** - Auto-close stale issues and PRs

### Enhancements to Existing Workflows

- [ ] **OS matrix testing** - Test on ubuntu/macos/windows in CI workflows
- [x] **Pre-commit integration** - Run pre-commit hooks in CI (`python-precommit.yml`)
- [ ] **SBOM generation** - Generate Software Bill of Materials for compliance
- [ ] **SLSA provenance** - Supply chain security attestations
- [ ] **Benchmark workflows** - Performance regression testing
- [ ] **API breaking change detection** - For library projects
- [ ] **Documentation builds** - mkdocs/sphinx builds and deployment

### Configuration Files

- [ ] **renovate.json** - Alternative to Dependabot with more features
- [ ] **.pre-commit-config.yaml** - Standard pre-commit hooks template
