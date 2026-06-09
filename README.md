# github-workflows

Reusable GitHub Actions workflows shared across my personal projects.

## Available Workflows

### Go CI

Path:

```text
.github/workflows/go-ci.yml
```

Usage:

```yaml
jobs:
  ci:
    uses: Rafael24595/github-workflows/.github/workflows/go-ci.yml@main
```

#### Inputs

| Name | Type | Default | Description |
| :-- | :-- | :-- | :-- |
| go-version | string | 1.25.5  | Go version used by the workflow |

#### Jobs Included

* Build
* Test
* Lint (golangci-lint)

## Versioning

Workflows can be referenced directly from the `main` branch:

```yaml
uses: Rafael24595/github-workflows/.github/workflows/go-ci.yml@main
```

## Purpose

This repository centralizes GitHub Actions workflows so they can be reused across multiple repositories. Updating a workflow here automatically makes the changes available to all projects that consume it.
