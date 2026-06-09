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

---

### Go Release

Path:

```text
.github/workflows/go-release.yml
```

Usage:

```yaml
jobs:
  release:
    uses: Rafael24595/github-workflows/.github/workflows/go-release.yml@main
    with:
      go-version: "1.25.5"
      artifact-name: "my-library"
```

#### Inputs

| Name | Type | Default | Description |
| :-- | :-- | :-- | :-- |
| go-version | string | 1.25.5 | Go version used by the workflow |
| artifact-name | string | release | Name of the generated release artifact |

#### Features

* Version validation from `go.package.yml`
* Semantic version enforcement
* Automatic timestamping of development versions
* Branch validation (`main` and `dev`)
* Git tag creation
* GitHub Release creation
* Source archive generation using `git archive`
* Go module testing

#### Version File

The workflow expects a `go.package.yml` file containing the project version:

```yaml
project:
  version: v1.2.3
```

Supported formats:

* `v1.2.3`
* `v1.2.3-beta.1`
* `v1.2.3-dev.0`

Development versions ending in `-dev.0` are automatically converted into timestamped versions during the release process.

#### Branch Rules

| Branch | Allowed Release Type |
| :--| :-- |
| main | Stable releases |
| dev | Development releases |

Releases triggered from invalid branches will fail.

#### Release Output

Each release generates:

* A Git tag
* A GitHub Release
* A source archive named:

```text
<artifact-name>-<version>.tar.gz
```

Example:

```text
my-library-v1.2.3.tar.gz
```

## Versioning

Workflows can be referenced directly from the `main` branch:

```yaml
uses: Rafael24595/github-workflows/.github/workflows/go-ci.yml@main
```

## Purpose

This repository centralizes GitHub Actions workflows so they can be reused across multiple repositories. Updating a workflow here automatically makes the changes available to all projects that consume it.
