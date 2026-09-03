# RL Check GitHub Action

This action runs `rl check` on your repository using the [RL language](https://github.com/rl-lang/rl-lang).

## Usage

Add this to your workflow:

```yaml
steps:
  - name: Run RL check
    uses: rl-lang/rl-check@v2
    with:
      file: 'src/main.rl'  # Required if 'folder' is not set
```

> **Note:** you no longer need a separate `actions/checkout` step just for this action, but you'll
> still want one in your workflow so there's a repo to check in the first place.

## Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `version` | `rl-cli` version to install from crates.io (e.g. `0.2.1`). Omit for the latest published version. | No | `''` (latest) |
| `file` | File to check (e.g., `main.rl`). Ignored if `folder` is set. | No* | `''` |
| `folder` | Folder to check (e.g., `nano/`, `cd/`). Runs `rl check` on every `.rl` file directly inside it. | No* | `''` |
| `args` | Additional arguments to pass to `rl check` | No | `''` |
| `working-directory` | Directory to run `rl check` in | No | `.` |

\* One of `file` or `folder` is required.

## Examples

### Check a single file
```yaml
- name: Run RL check
  uses: rl-lang/rl-check@v2
  with:
    file: 'main.rl'
```

### Check every file in a folder
```yaml
- name: Run RL check on a folder
  uses: rl-lang/rl-check@v2
  with:
    folder: 'src/'
```

### With arguments
```yaml
- name: Run RL check with options
  uses: rl-lang/rl-check@v2
  with:
    file: 'src/main.rl'
    args: '--verbose'
```

### Specific version
```yaml
- name: Run RL check with a specific rl-cli version
  uses: rl-lang/rl-check@v2
  with:
    version: '0.1.4'
    file: 'main.rl'
```

### Different working directory
```yaml
- name: Run RL check in subdirectory
  uses: rl-lang/rl-check@v2
  with:
    file: 'main.rl'
    working-directory: './src'
```

## Full example workflow

```yaml
name: CI

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  rl-check:
    name: RL Check
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Run RL check
        uses: rl-lang/rl-check@v2
        with:
          file: 'src/main.rl'
          args: '--verbose'
```

## Versioning

| Reference | Description |
|-----------|-------------|
| `@main` | Latest development version |
| `@v2` | Latest v2.x release |
| `@v2.0.0` | Specific version |
| `@v1` | Legacy version — installs a prebuilt binary from GitHub Releases |

### Migrating from v1

`v2` installs `rl-cli` via `cargo install` (with caching) instead of downloading a prebuilt binary
from GitHub Releases. This is a breaking change to the `version` input:

- **Before (`v1`):** `version: 'v0.1.4'`, or the magic values `latest` / `nightly`
- **Now (`v2`):** `version: '0.1.4'` (no `v` prefix, matching crates.io), or omit for latest.
  `nightly` is no longer supported, since crates.io has no nightly channel.

If you have `version` pinned in your workflow, drop the leading `v` when moving to `@v2`.

## License

[MIT](LICENSE)
