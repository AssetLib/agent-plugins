# AGENTS.md

Public repository `AssetLib/agent-plugins`: a plugin marketplace named `assetlib` for Claude Code and Codex. It ships one skills-only plugin, `assetlib-audit`, which runs the published npm package `@assetlib/audit` and explains the result. The audit itself is developed in [AssetLib/sdk-js](https://github.com/AssetLib/sdk-js) under `packages/audit`. This repository has no build, no package manifest, and no CI: it is JSON manifests, Markdown, and icons.

## Commands

Validate before calling any change done. Both must exit 0:

```sh
claude plugin validate .                          # marketplace plus every plugin it lists
claude plugin validate ./plugins/assetlib-audit   # the plugin alone
```

Both currently pass with five warnings about fields Claude Code does not read in `plugins/assetlib-audit/.claude-plugin/plugin.json`: `icon`, `documentationUrl`, `supportUrl`, `privacyPolicyUrl`, `termsOfServiceUrl`. They are kept on purpose for directory listings, so `--strict` fails today. Do not delete them to make it pass. Any other warning or error is yours to fix.

Other read-only checks:

```sh
claude plugin tag --dry-run ./plugins/assetlib-audit   # plugin.json and the marketplace entry agree
npx -y @assetlib/audit@0.1.0 --help                     # the published CLI the skill pins
```

Try the plugin locally:

```sh
claude --plugin-dir ./plugins/assetlib-audit   # Claude Code, this session only
codex plugin marketplace add .                 # Codex: adds this checkout to your Codex config
codex plugin add assetlib-audit@assetlib
codex plugin marketplace remove assetlib       # undo
```

## Layout

- `.claude-plugin/marketplace.json`: the Claude Code marketplace `assetlib`, with each plugin's description, category, and tags.
- `.agents/plugins/marketplace.json`: the same marketplace for Codex (source path, install policy, category).
- `plugins/assetlib-audit/.claude-plugin/plugin.json`: Claude Code manifest.
- `plugins/assetlib-audit/plugin.json`: portable manifest Codex reads. `extensions["com.openai"].interface` holds the OpenAI directory copy, icon, and default prompt.
- `plugins/assetlib-audit/skills/assetlib-audit/SKILL.md`: the skill. Its frontmatter `name` must equal the directory name. `agents/openai.yaml` beside it holds Codex display metadata and `allow_implicit_invocation`.
- `plugins/assetlib-audit/assets/`: `icon.svg` (Claude manifest) and `icon.png` (Codex manifest).

## Things that change together

- **Pinned audit version.** `@assetlib/audit@0.1.0` appears in `SKILL.md` (Run section) and `plugins/assetlib-audit/README.md`. The same pin lives in sdk-js: `packages/audit/package.json`, its README, and the CLI help text in `packages/audit/bin/assetlib-audit.mjs`. On every audit release, move all of them to the new exact version in the same pass. Pin only versions already published on npm. Never use a range, `latest`, or a different package.
- **Plugin version.** `version` in `.claude-plugin/plugin.json` and `plugin.json` must match. Claude Code uses it to decide whether installed copies update, so bump it with any change users should receive, including an audit pin bump. `.claude-plugin/marketplace.json` has its own top-level `version` for the marketplace.
- **Listing copy.** The plugin `description` is identical in both plugin manifests and the Claude marketplace entry. Manifest `keywords` match the marketplace `tags`. `defaultPrompt` and `brandColor` in `plugin.json` match `default_prompt` and `brand_color` in `agents/openai.yaml`. Privacy and terms URLs appear in both manifests.
- **Facts about the audit.** The plugin README, `SKILL.md`, and the Codex `longDescription` state what the audit reads (PNG, JPEG, WebP), what it needs (Node.js 22 or later), and what it fetches (the package and its one dependency). If the audit package changes any of these, update all three.
- **Install commands.** The root README and `plugins/assetlib-audit/README.md` show the same install steps. Codex needs two commands, `codex plugin marketplace add AssetLib/agent-plugins` then `codex plugin add assetlib-audit@assetlib`; adding the marketplace alone installs nothing (checked with codex-cli 0.153.4 on 2026-10-09).
- **New plugins** need an entry in both marketplace files, both manifests, and a row in the root README table.

## Invariants

- **The skill is read-only.** It runs the audit and reports. It never deletes, uploads, transforms, or rewrites the user's assets or source, and never makes cleanup, publishing, analytics, or a repository-wide rewrite a side effect of an audit. Migration proposals keep every current image as a bundled fallback.
- **`assetlib-audit` stays skills-only.** No hooks, MCP server, scripts, or network calls of its own; its README promises this. Directories cannot add an MCP server to a published skills-only plugin later, so connected tools (MCP plus OAuth) will ship as a separate plugin. The bare name `assetlib` is reserved for that plugin.
- **Names and paths are public contracts.** Marketplace `assetlib`, plugin and skill `assetlib-audit`, path `plugins/assetlib-audit`, install id `assetlib-audit@assetlib`, and `$assetlib-audit` in the default prompts. Users install by these, and Claude and OpenAI directory listings (pending review as of 2026-10-09) reference them. Renaming or moving any of them breaks installs.
- **No invented integration.** The skill must not invent Assetlib SDK imports, package names, API keys, an MCP server, or installation success. Connected features are planned; describe them as planned.

## Writing rules

- The product is "Assetlib" in prose. `AssetLib` is only the GitHub org slug.
- Plain, specific, verifiable. Every claim about the audit must match its `--help` output and behavior. An unmatched reference is "unresolved", never "unused".
- Avoid "instantly", "every screen", "zero engineering", "automatically unused". No invented customers, metrics, or testimonials. No emojis.

## Release

There is no package to publish. Users receive whatever is on `main` through the marketplace. No release tags exist yet (as of 2026-10-09). A release is: update the audit pin and plugin version as above, run both `claude plugin validate` commands, and merge to `main`. `claude plugin tag` can create `assetlib-audit--v<version>` tags; ask the maintainer before creating or pushing one.

## Don'ts

- Don't change audit behavior here; it lives in AssetLib/sdk-js.
- Don't add hooks, MCP servers, scripts, or dependencies to `assetlib-audit`, and don't use the name `assetlib` for a plugin.
- Don't rename or move plugins, skills, the marketplace, or plugin paths.
- Don't remove the intentionally unread manifest fields to satisfy `--strict`.
- Don't commit secrets, tokens, or absolute local paths.
- Fix a confirmed defect you find in the same change and verify it. Defer only when a fix is unsafe now, and say when it will happen.
