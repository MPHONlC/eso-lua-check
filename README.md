# Lua Check

Syntax-checks every `.lua` file in your project (`luac5.1 -p`) and runs real static analysis (`luacheck`, via the actual [lunarmodules/luacheck](https://github.com/lunarmodules/luacheck) Docker image) - findings are annotated directly on the offending file and line, and a job summary is generated either way.

This is a thin wrapper: it uses `ghcr.io/lunarmodules/luacheck` for the actual linting, and simply adds reporting on top (line-level annotations, a step summary, `luac5.1` syntax checking alongside it). All credit for `luacheck` itself belongs to its own authors.

## Usage

Your workflow must check out the repo first. A `.luacheckrc` at your repo root (with `std = "lua51"` and whatever `ignore`/`globals` your project's real API surface needs) is picked up automatically by `luacheck` - see [lunarmodules/luacheck's docs](https://luacheck.readthedocs.io/) for how to write one.

```yaml
name: Lua Check

on:
  workflow_dispatch:
  workflow_call:

jobs:
  lua-check:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v7
      - uses: MPHONlC/lua-check@Version-0.0.3
```

## Inputs

| Input | Required | Default | Description |
|---|---|---|---|
| `luacheck_image` | No | `ghcr.io/lunarmodules/luacheck:v1.2.0` | Override to pin a different luacheck version. |

## Requirements

- `docker` must be available on the runner (true by default on GitHub-hosted `ubuntu-latest`).
- A `.luacheckrc` at the repo root, tuned to your project's actual API surface.

> [!WARNING]
> Without a `.luacheckrc` tuned to your project's real globals, every one of them shows up as an "accessing undefined variable" warning. See [lunarmodules/luacheck's docs](https://luacheck.readthedocs.io/) for how to write one.

## License

MIT - see [LICENSE](LICENSE). `luacheck` itself is a separate project under its own license - see [lunarmodules/luacheck](https://github.com/lunarmodules/luacheck).
