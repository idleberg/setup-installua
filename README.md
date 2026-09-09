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
    nsis-version: "latest"
    large-strings: false
    advanced-logging: false
```

## Inputs

| Name               | Default  | Description                                                     |
| ------------------ | -------- | --------------------------------------------------------------- |
| `version`          | `latest` | Installua version, e.g. `0.1.0`. `latest` resolves via crates.io. |
| `nsis-version`     | `latest` | NSIS version, `3.12` or newer. `latest` resolves via SourceForge. |
| `large-strings`    | `false`  | `NSIS_MAX_STRLEN=8192`.                                         |
| `advanced-logging` | `false`  | `NSIS_CONFIG_LOG=yes`.                                          |

> [!NOTE]
> Installua targets NSIS 3.12, so pinning `nsis-version` to anything older
> fails the step rather than letting `makensis` fail later with an error that
> does not name the real cause.

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

## License

[Apache License, Version 2.0](LICENSE)
