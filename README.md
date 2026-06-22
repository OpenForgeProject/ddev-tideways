[![add-on registry](https://img.shields.io/badge/DDEV-Add--on_Registry-blue)](https://addons.ddev.com)
[![tests](https://github.com/OpenForgeProject/ddev-tideways/actions/workflows/tests.yml/badge.svg?branch=main)](https://github.com/OpenForgeProject/ddev-tideways/actions/workflows/tests.yml?query=branch%3Amain)
[![last commit](https://img.shields.io/github/last-commit/OpenForgeProject/ddev-tideways)](https://github.com/OpenForgeProject/ddev-tideways/commits)
[![release](https://img.shields.io/github/v/release/OpenForgeProject/ddev-tideways)](https://github.com/OpenForgeProject/ddev-tideways/releases/latest)

# DDEV Tideways

Integrates the [Tideways](https://tideways.com/) PHP profiler into a [DDEV](https://ddev.com/) project. It installs the Tideways PHP extension and CLI into the `web` container, runs the Tideways daemon as a service, and configures everything through environment variables.

## Installation

```bash
ddev add-on get OpenForgeProject/ddev-tideways
ddev dotenv set .ddev/.env.tideways --tideways-apikey=YOUR_API_KEY
ddev restart
```

Find your API key in your Tideways project settings. The key is stored in `.ddev/.env.tideways` — commit or git-ignore that file per your team's policy.

## Configuration

Change any value with `ddev dotenv set .ddev/.env.tideways --<flag>=<value>` followed by `ddev restart`:

| Variable | Flag | Default |
| -------- | ---- | ------- |
| `TIDEWAYS_APIKEY` | `--tideways-apikey` | _(empty)_ |
| `TIDEWAYS_ENVIRONMENT` | `--tideways-environment` | `ddev` |
| `TIDEWAYS_SERVICE` | `--tideways-service` | `app` |
| `TIDEWAYS_TRACE_SAMPLE_RATE` | `--tideways-trace-sample-rate` | `100` |
| `TIDEWAYS_CLI_TOKEN` | `--tideways-cli-token` | _(empty)_ |
| `TIDEWAYS_DAEMON_DOCKER_IMAGE` | `--tideways-daemon-docker-image` | `ghcr.io/tideways/daemon:latest` |

`TIDEWAYS_CONNECTION` is fixed to `tcp://tideways-daemon:9135`.

## Usage

```bash
ddev tideways-doctor status   # extension, CLI, daemon and configuration
ddev tideways-doctor test     # generate traffic and confirm data is collected
ddev tideways-doctor logs -f  # follow the daemon logs
```

`ddev tideways …` passes through to the Tideways CLI for profiling console scripts. Import your CLI settings once (token from `https://app.tideways.io/user/cli-import-settings`), then trace a command:

```bash
ddev tideways import <token>                       # re-run after a web image rebuild
ddev tideways run [project] php bin/magento cron:run
```

## No data in the dashboard?

Tideways silently discards data when the target does not exist within your account limits:

* **Environment** — `TIDEWAYS_ENVIRONMENT` must be an environment that exists in your project.
* **Service** — a service beyond your plan's service limit is discarded; `app` is Tideways' default service, so keep it unless you have a free service slot.

## Removal

```bash
ddev add-on remove tideways
```

## Credits

Contributed and maintained by [@OpenForgeProject](https://github.com/OpenForgeProject)
