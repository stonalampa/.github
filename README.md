# stonalampa/.github

Shared GitHub configuration for my repositories.

## Reusable workflows

### Go CI — `go-ci.yml`

Runs the standard checks for a Go repo through its Makefile: `make vet`, `make test`, `make lint`, `make vuln`. Go is set up from the caller's `go.mod`; golangci-lint is installed by the official action.

Call it from a repo's `.github/workflows/ci.yml`:

```yaml
name: CI

on:
  push:
    branches: [main]
  pull_request:

permissions:
  contents: read

jobs:
  ci:
    uses: stonalampa/.github/.github/workflows/go-ci.yml@v1
```

The status check is named `ci / check`; require that name in branch protection.

Callers pin a major tag (`@v1`), never `@main`, so a change here cannot break every repo at once. Breaking changes get a new major tag (`v2`), and each repo moves deliberately.

The caller's repo must provide the Makefile targets `vet`, `test`, `lint` and `vuln`.
