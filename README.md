# rl-check

Runs `rl check` on `.rl` files.

```yaml
- uses: rl-lang/rl-check@main
  with:
    file: src/main.rl   # or folder: src/
```

| Input | Default |
|---|---|
| `version` | `latest` |
| `file` | `''` |
| `folder` | `''` |
| `args` | `''` |
| `working-directory` | `.` |

One of `file` or `folder` is required.
