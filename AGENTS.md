# AGENTS.md — GitHub Gist for TRMNL

A [TRMNL](https://usetrmnl.com) plugin that fetches a GitHub Gist and renders its content as markdown on an e-ink display.

## Quick commands

```bash
cp .env.example .env    # edit with real values
bin/trmnlp lint         # validate plugin config & templates
bin/trmnlp preview      # local preview (Docker or native gem)
bin/trmnlp push --force # deploy to TRMNL (CI on main branch)
```

## Environment (`.env`, gitignored)

| Var | Purpose |
|---|---|
| `TRMNL_GIST_ID` | Gist hex ID from the URL |
| `TRMNL_GIST_TOKEN` | GitHub PAT with `gist` scope |

`bin/trmnlp` auto-sources `.env` on every invocation.

## Architecture

- **`src/settings.yml`** — plugin definition: polling URL, headers, custom fields, refresh interval
- **`src/shared.liquid`** — fetches gist via `polling_url`, parses markdown with `markdown_to_html` filter
- **`.trmnlp.yml`** — TRMNLP tool config (watch paths, custom field variable bindings)

Templates use Liquid + TRMNL-specific filters (`parse_json`, `markdown_to_html`).

## CI / Deploy

GitHub Actions workflow (`.github/workflows/trmnl.yml`):
1. **lint** — `gem install trmnl_preview` then `trmnlp lint`
2. **push** (main only, after lint) — `trmnlp push --force` with `TRMNL_API_KEY` secret

## Tooling

- `bin/trmnlp` tries native `trmnlp` CLI first, falls back to Docker (`trmnl/trmnlp`).
- Without either, install via `gem install trmnl_preview`.
- Build artifacts go to `_build/` (gitignored).
