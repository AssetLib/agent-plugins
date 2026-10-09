# Assetlib agent plugins

Plugins that let coding agents use Assetlib. Install from this repository as a marketplace, or wait for the directory listings.

| Plugin | What it does | Status |
| --- | --- | --- |
| [`assetlib-audit`](plugins/assetlib-audit) | Runs the read-only `@assetlib/audit` command on an app and explains duplicate, dimension, and reference evidence before any migration | Skills only, no account needed |

Connected tools for inspecting placements and preparing releases in a hosted Assetlib workspace are planned as a separate plugin once the hosted API exposes authenticated operations. They are not in this repository yet.

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

## Develop

Each plugin lives under `plugins/<name>` with its own manifest, README, and license. The Claude manifest is in `.claude-plugin/plugin.json`; the portable manifest Codex reads is `plugin.json` at the plugin root. Validate before opening a pull request:

```sh
claude plugin validate .
claude plugin validate ./plugins/assetlib-audit
```

The audit command itself is developed in [AssetLib/sdk-js](https://github.com/AssetLib/sdk-js). Bump the pinned version in the skill when a new audit version is published.

## License

MIT.
