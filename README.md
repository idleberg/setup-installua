# setup-installua

![License](https://img.shields.io/github/license/idleberg/setup-installua?color=blue&style=for-the-badge)
![Release](https://img.shields.io/github/v/release/idleberg/setup-installua?style=for-the-badge)
![CI](https://img.shields.io/github/actions/workflow/status/idleberg/setup-installua/ci.yml?style=for-the-badge)

Set up [Installua](https://github.com/idleberg/installua) in your GitHub workflow.

## Usage

```yaml
- uses: idleberg/setup-installua@v0
- run: installua build setup.lua
```

With build options:

```yaml
- uses: idleberg/setup-installua@v0
  with:
    version: "latest"
    large-strings: false
    advanced-logging: false
```

> [!TIP]
> Pin the action to a full commit SHA rather than a tag. A tag can be moved to
> point at different code; a SHA cannot.

## Inputs

| Name               | Default  | Description                      |
| ------------------ | -------- | -------------------------------- |
| `version`          | `latest` | Installua version, e.g. `0.1.0`. |
| `large-strings`    | `false`  | `NSIS_MAX_STRLEN=8192`.          |
| `advanced-logging` | `false`  | `NSIS_CONFIG_LOG=yes`.           |

> [!NOTE]
> On Windows, `large-strings` and `advanced-logging` cannot be combined — see
> [setup-nsis](https://github.com/nsis-dev/setup-nsis) for why. Enabling both
> fails the step.

## Outputs

| Name           | Description                                               |
| -------------- | --------------------------------------------------------- |
| `version`      | Resolved Installua version.                               |
| `nsis-version` | Resolved NSIS version.                                    |
| `nsisdir`      | NSIS installation directory (also exported as `NSISDIR`). |

Installua compiles to NSIS and hands the result to `makensis`, so this action
runs [setup-nsis](https://github.com/nsis-dev/setup-nsis) first, then builds the
Installua CLI from crates.io. Both are cached per OS, version and option
combination, so only the first run pays the setup cost.

The NSIS version is not yours to choose: every Installua release targets the
latest NSIS, so that is what gets installed. The `nsis-version` output tells you
which one that was.

## License

[Apache License, Version 2.0](LICENSE)
