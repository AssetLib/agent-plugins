# Assetlib agent plugins

Plugins that let coding agents use Assetlib. Install from this repository as a marketplace, or wait for the directory listings.

| Plugin | What it does | Status |
| --- | --- | --- |
| [`assetlib-audit`](plugins/assetlib-audit) | Runs the read-only `@assetlib/audit` command on an app and explains duplicate, dimension, and reference evidence before any migration | Skills only, no account needed |
| [`assetlib`](plugins/assetlib) | Connects the agent to an Assetlib workspace through the hosted MCP server: reads what each placement shows in draft, staging and production, and prepares image changes as a draft. Publishing stays with a person | Needs an Assetlib account; sign in in the browser |

## Install

Claude Code:

```sh
claude plugin marketplace add AssetLib/agent-plugins
claude plugin install assetlib-audit@assetlib
claude plugin install assetlib@assetlib
```

After installing `assetlib`, run `/mcp`, choose `assetlib` and Authenticate.

Codex CLI:

```sh
codex plugin marketplace add AssetLib/agent-plugins
codex plugin add assetlib-audit@assetlib
codex plugin add assetlib@assetlib
```

In Codex, connect the workspace by adding the server and signing in: `codex mcp add assetlib --url https://console.assetlib.dev/mcp`, then `codex mcp login assetlib`.

Other agents, such as Cursor or GitHub Copilot, through the [skills CLI](https://github.com/vercel-labs/skills). It installs the same skill without the plugin wrapper:

```sh
npx skills add AssetLib/agent-plugins --skill assetlib-audit
```

They connect to a workspace by adding the MCP server `https://console.assetlib.dev/mcp` (remote, with OAuth), for example `claude mcp add --transport http assetlib https://console.assetlib.dev/mcp` or, in Cursor, `{"mcpServers":{"assetlib":{"url":"https://console.assetlib.dev/mcp"}}}` in `.cursor/mcp.json`.

## Develop

Each plugin lives under `plugins/<name>` with its own manifest, README, and license. The Claude manifest is in `.claude-plugin/plugin.json`; the portable manifest Codex reads is `plugin.json` at the plugin root. Validate before opening a pull request:

```sh
claude plugin validate .
claude plugin validate ./plugins/assetlib-audit
claude plugin validate ./plugins/assetlib
```

The audit command itself is developed in [AssetLib/sdk-js](https://github.com/AssetLib/sdk-js). Bump the pinned version in the skill when a new audit version is published.

## License

MIT.
