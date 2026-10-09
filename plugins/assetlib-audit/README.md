# Assetlib Audit plugin

A skills-only plugin for Claude Code and Codex that teaches the agent to run Assetlib's read-only artwork audit on an app and interpret the result honestly.

## What it does

When you ask the agent to audit, inventory, dedupe, or shrink an app's images, or to assess moving artwork to remote delivery, the skill runs the pinned command `npx -y @assetlib/audit@0.1.0 <app-root> --json` and explains the evidence: measured bytes for every PNG, JPEG, and WebP file, exact duplicate groups by SHA-256, dimension candidates above a configurable long edge, and optional quoted-reference hints with source locations. It then proposes at most a few artwork placements for a bounded migration and keeps every current image as a bundled fallback. Recommending no migration is a normal outcome.

## What it runs, sends, and fetches

- Runs: the `@assetlib/audit` command-line tool from npm, pinned to an exact version, with Node.js 22 or later. Source is MIT-licensed at https://github.com/AssetLib/sdk-js.
- Fetches: the package and its one dependency from the npm registry on first run. Nothing else.
- Sends: nothing. The audit has no account, telemetry, upload, or network calls. It writes no files and changes nothing in the audited app.

The plugin contains no hooks, no MCP server, and no scripts of its own. It is instructions plus an icon.

## Install

Claude Code:

```sh
claude plugin marketplace add AssetLib/agent-plugins
claude plugin install assetlib-audit@assetlib
```

Codex CLI:

```sh
codex plugin marketplace add AssetLib/agent-plugins
codex plugin add assetlib-audit@assetlib
```

Then ask, for example: "Audit the images in this app and tell me what could move to remote delivery without losing the offline fallback."

## Limits

The audit covers PNG, JPEG, and WebP files only. It does not parse asset catalogs, Android resource names, SVG, GIF, HEIC, fonts, or video. An unmatched literal reference means unresolved, not unused. This plugin does not connect to a hosted Assetlib workspace, publish releases, or install a delivery SDK. Those capabilities will arrive as a separate connected plugin once the hosted API exposes them.

## License

MIT. See LICENSE in this folder.
