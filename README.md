# GitHub Gist for TRMNL

A [TRMNL](https://usetrmnl.com) plugin that fetches content from a GitHub Gist and renders it as markdown on an e-ink display.

Template Gist: https://gist.github.com/dnnsmnstrr/77ce7da8d53013984f13cf5802a8b95a

## Setup

```bash
cp .env.example .env
```

Edit `.env` with your values:

| Variable | Description |
|---|---|
| `TRMNL_GIST_ID` | Gist hex ID from the gist URL |
| `TRMNL_GIST_FILE_NAME` | Filename inside the gist (e.g. `trmnl.md`) |
| `TRMNL_GIST_TOKEN` | GitHub personal access token with `gist` scope |

## Usage

```bash
bin/trmnlp lint         # validate plugin
bin/trmnlp serve      # local preview
bin/trmnlp push --force # deploy to your TRMNL device
```

The `bin/trmnlp` wrapper sources `.env` automatically and uses the native `trmnlp` CLI, falling back to Docker (`trmnl/trmnlp`) if unavailable.

## Deploy

Pushes to `main` trigger CI: lint → push to TRMNL. Requires `TRMNL_API_KEY` in repo secrets.

## How it works

A polling plugin that calls the [GitHub Gist API](https://docs.github.com/en/rest/gists/gists), parses the gist file's markdown content, and displays it on your TRMNL screen.
