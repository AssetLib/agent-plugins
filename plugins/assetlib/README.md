# Assetlib plugin

Connects Claude Code or Codex to an Assetlib workspace through Assetlib's hosted MCP server, so the agent can see what each image placement in your app shows and prepare image changes as a draft. Assetlib updates a mobile app's images without an app release; publishing stays with a person in the console.

## What it does

After you sign in, the agent can:

- read each placement's contract (key, screen, size, states, variants), the image it shows in draft, staging and production, its code references and small previews;
- read the image library, release history, and the workspace's setup checklist and SDK setup steps;
- with your permission, upload an image and assign images to placements as a draft, then give you the console link to review and publish.

It cannot publish, promote, restore, approve, create placements, or change members, tokens or settings. Placements come from your app's `assetlib.catalog.json`, registered by `assetlib sync` in CI.

## What it connects to and sends

- Connects to: `https://console.assetlib.dev/mcp`, Assetlib's hosted MCP server, over HTTPS. The plugin contains no scripts or hooks of its own; it is this server address and a skill.
- Signs in: in your browser, with your Assetlib account (GitHub or Google). You choose one workspace and whether the agent may draft changes. The agent acts as you, with your role in that workspace. Disconnect it any time in the console under Profile › Agent connections.
- Sends: what the agent asks for in tool calls, and image files you have it upload. Reading a workspace is free; uploads count toward the workspace's storage like any upload ([pricing](https://www.assetlib.dev/pricing/)).

## Install

Claude Code:

```sh
claude plugin marketplace add AssetLib/agent-plugins
claude plugin install assetlib@assetlib
```

Then run `/mcp`, choose `assetlib` and Authenticate.

Codex CLI:

```sh
codex plugin marketplace add AssetLib/agent-plugins
codex plugin add assetlib@assetlib
codex mcp login assetlib
```

Without the plugin, add the server directly:

```sh
claude mcp add --transport http assetlib https://console.assetlib.dev/mcp
```

```sh
codex mcp add assetlib --url https://console.assetlib.dev/mcp
```

Cursor and other MCP clients that support remote servers with OAuth: add `https://console.assetlib.dev/mcp`.

Then ask, for example: "What does each Assetlib placement on the Discover screen show in production?"

## License

MIT. See LICENSE in this folder.
