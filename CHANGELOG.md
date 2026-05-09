# Changelog — forge-cli

All notable changes to the `forge-cli` module are documented here.

Format: [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).
Versioning: [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [0.6.0] — 2026-05-09

Media subcommands and AVIF support (Milestone 13, Amendment A93).

### Added

- `forge-cli media upload <file> [--description <text>]` — uploads a file to
  the Forge media library via `POST /media` with the configured bearer token.
  `--description` is required for image files (WCAG 1.1.1). Prints the returned
  URL on success.
- `forge-cli media list [--type image|document|video|audio|other]` — lists all
  media records. Prints a table of ID, type, upload date, and URL.
- `forge-cli media delete <id>` — permanently deletes a media record by ID.
- `.avif` added to the image extension set — AVIF uploads now require
  `--description`, consistent with forge-media v1.2.0 AVIF support.

---

## [0.5.0] — 2026-05-08

Draft preview subcommand (Milestone 12, Amendment A92).

### Added

- `forge preview <prefix> <slug>` — generates a signed draft preview URL via the
  `create_preview_url` MCP tool and prints it to stdout. Requires Admin role.
  The URL grants read access to Draft or Scheduled content for the token lifetime
  (default 12 h). Archived items return 404 even with a valid token.

---

## [0.4.0] — 2026-05-08

Webhook management commands (Milestone 11 — CLI parity for forge-mcp webhook tools).

### Added

- `forge webhook create --url URL --events EVENT,...` — registers a new outbound
  webhook endpoint (HTTPS only). Prints the signing secret once.
- `forge webhook list` — lists all registered endpoints.
- `forge webhook delete <endpoint-id>` — removes an endpoint by ID.
- `forge webhook deliveries <job-id>` — shows delivery log for a job.
- `forge webhook retry <job-id>` — re-queues a dead job for delivery.

---

## [0.3.0] — 2026-05-04

### Added

- `forge-cli init [--url URL] [--bootstrap-token TOKEN] [--name NAME] [--days N] [--force]`
  Bootstrap a new Forge instance: validates reachability (`/_health`), creates
  a named admin token via the bootstrap token, writes `.forge-cli.env`
  (`FORGE_URL` + `FORGE_TOKEN`), and verifies the new token. Use `--force` to
  overwrite an existing env file.

---

## [0.2.1] — 2026-05-02

Patch release — no code changes. Re-tag to refresh module proxy cache after
vanity URL migration to `forge-cms.dev`.

---

## [0.2.0] — 2026-04-30

Go 1.26.2 and module path migration to `forge-cms.dev` (Amendment A76).

### Changed

- `go.mod`: module path renamed from `github.com/forge-cms/forge-cli` to
  `forge-cms.dev/forge-cli`; `go` directive bumped from `1.22` to `1.26.2`.

---

## [0.1.0] — 2026-04-07

Initial release — operator CLI for Forge instances (Decision 28).

### Added

- `forge-cli <type> create [--from file]` — create a Draft via `POST /{prefix}`
- `forge-cli <type> update <slug> [--from file]` — GET-then-PUT field overlay
- `forge-cli <type> publish <slug>` — GET-then-PUT with `Status: published`
- `forge-cli <type> unpublish <slug>` — GET-then-PUT with `Status: draft`
- `forge-cli <type> archive <slug>` — GET-then-PUT with `Status: archived`
- `forge-cli <type> delete <slug>` — `DELETE /{prefix}/{slug}`
- `forge-cli <type> list [--status <s>]` — list items with optional status filter
- `forge-cli <type> get <slug>` — print a single item as JSON
- `forge-cli token create --name <n> --role <r> [--ttl <d>]` — issue a token via MCP
- `forge-cli token list` — list tokens via MCP
- `forge-cli token revoke <id>` — revoke a token via MCP
- `forge-cli status` — `GET /_health`, print JSON
- Config via `FORGE_URL`, `FORGE_TOKEN`, `FORGE_MCP_URL` env vars or `.forge-cli.env`
- YAML-subset frontmatter parser (no external dependencies)
- Pure stdlib — zero third-party dependencies
