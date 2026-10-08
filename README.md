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
    with:
      test-tags: integration
```

#### Inputs

| Name       | Type   | Default | Description                                |
| :--------- | :----- | :------ | :----------------------------------------- |
| go-version | string | 1.25.5  | Go version used by the workflow            |
| test-tags  | string | ""      | Optional Go build tags passed to `go test` |

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

| Name          | Type   | Default | Description                            |
| :------------ | :----- | :------ | :------------------------------------- |
| go-version    | string | 1.25.5  | Go version used by the workflow        |
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
| :----- | :------------------- |
| main   | Stable releases      |
| dev    | Development releases |

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

---

### Shell CI

Path:

```text
.github/workflows/sh-ci.yml
```

Usage:

```yaml
jobs:
  ci:
    uses: Rafael24595/github-workflows/.github/workflows/sh-ci.yml@main
```

---

### Zig CI

Path:

```text
.github/workflows/zig-ci.yml
```

Usage:

```yaml
jobs:
  ci:
    uses: Rafael24595/github-workflows/.github/workflows/zig-ci.yml@main
    with:
      zig-version: "0.15.1"
```

#### Inputs

| Name        | Type   | Default | Description                      |
| :---------- | :----- | :------ | :------------------------------- |
| zig-version | string | 0.15.1  | Zig version used by the workflow |

#### Jobs Included

* Build
* Test

The workflow runs:

```text
zig build
zig build test
```

---

### Zig Release

Path:

```text
.github/workflows/zig-release.yml
```

Usage:

```yaml
jobs:
  release:
    uses: Rafael24595/github-workflows/.github/workflows/zig-release.yml@main
    with:
      zig-version: "0.15.1"
```

#### Inputs

| Name        | Type   | Default | Description                      |
| :---------- | :----- | :------ | :------------------------------- |
| zig-version | string | 0.15.1  | Zig version used by the workflow |

#### Features

* Artifact name extraction from `build.zig.zon`
* Semantic version validation
* Git tag creation
* Multi-platform executable builds
* Linux x86_64 executable
* Windows x86_64 executable
* SHA-256 checksum generation
* GitHub Release creation
* Automatic GitHub source archives

#### Version File

The workflow expects the project version to be defined in `build.zig.zon`:

```zig
.version = "1.2.3",
```

Supported formats:

* `1.2.3`
* `1.2.3-beta.1`
* `1.2.3-dev.0`

The version is used as the Git tag and GitHub Release version.

#### Artifact Name

The workflow extracts the project name from `build.zig.zon`:

```zig
.name = .my_program,
```

The extracted name is used as the base name for the generated executables.

#### Release Output

Each release generates:

* A Git tag
* A GitHub Release
* A Linux x86_64 executable
* A Windows x86_64 executable
* A SHA-256 checksums file

Example:

```text
my_program-linux-x86_64
my_program-windows-x86_64.exe
checksums.txt
```

GitHub also automatically provides source archives for the release tag:

```text
Source code (zip)
Source code (tar.gz)
```

#### Inputs

This workflow does not take any input parameters.

#### Features

* Zero third-party dependencies (uses tools pre-installed on the runner)
* Automatic discovery of `.sh` scripts and executable shell files without extensions
* Syntax validation using `bash -n`
* Static code analysis using `shellcheck` with sourced file tracking (`-x`)

#### Jobs Included

* Lint (ShellCheck & Syntax)

## Versioning

Workflows can be referenced directly from the `main` branch:

```yaml
uses: Rafael24595/github-workflows/.github/workflows/go-ci.yml@main
```

## Purpose

This repository centralizes GitHub Actions workflows so they can be reused across multiple repositories. Updating a workflow here automatically makes the changes available to all projects that consume it.
