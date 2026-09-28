# setup-swamp

A GitHub Action to install [swamp](https://github.com/swamp-club/swamp) and
optionally authenticate with [swamp.club](https://swamp.club).

## Usage

```yaml
- uses: swamp-club/setup-swamp@v0.1.0
  with:
    api-key: ${{ secrets.SWAMP_API_KEY }}
```

### Inputs

| Input            | Required | Default  | Description                                        |
|------------------|----------|----------|----------------------------------------------------|
| `version`        | No       | `stable` | Swamp version to install                           |
| `api-key`        | No       |          | API key for authenticating with swamp.club         |
| `swamp-club-url` | No       |          | Override the swamp.club server URL                 |
| `repo-init`      | No       | `false`  | Run `swamp repo init` after setup                  |

### Outputs

| Output    | Description                             |
|-----------|-----------------------------------------|
| `version` | The version of swamp that was installed |

### Examples

**Install stable and authenticate:**

```yaml
steps:
  - uses: actions/checkout@v4
  - uses: swamp-club/setup-swamp@v0.1.0
    with:
      api-key: ${{ secrets.SWAMP_API_KEY }}
  - run: swamp model list
```

**Install a specific version:**

```yaml
steps:
  - uses: actions/checkout@v4
  - uses: swamp-club/setup-swamp@v0.1.0
    with:
      version: 20260928.150833.0-sha.6858940b
```

**Install, authenticate, and initialize the repo:**

```yaml
steps:
  - uses: actions/checkout@v4
  - uses: swamp-club/setup-swamp@v0.1.0
    with:
      api-key: ${{ secrets.SWAMP_API_KEY }}
      repo-init: true
  - run: swamp workflow run my-workflow
```

## How it works

1. Installs swamp using the official install script from
   `swamp-club.com/install.sh`
2. Adds swamp to `PATH`
3. If `api-key` is provided, masks the value in logs and sets `SWAMP_API_KEY` as
   an environment variable for all subsequent steps, then verifies authentication
   with `swamp auth whoami`
4. If `swamp-club-url` is provided, sets `SWAMP_CLUB_URL` as an environment
   variable for all subsequent steps
5. If `repo-init` is `true`, runs `swamp repo init`

## Supported platforms

- Linux (x86_64, aarch64)
- macOS (x86_64, aarch64)

## License

AGPLv3 - see [LICENSE](LICENSE) for details.
