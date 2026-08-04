---
description: >-
  Connect the GetBlock Docs MCP server to Claude, Claude Code, Cursor, VS Code,
  or ChatGPT and query the documentation directly from your AI assistant.
icon: mcp
---

# GetBlock Docs MCP server

The GetBlock documentation is available as a **Model Context Protocol (MCP) server** — a live, machine-readable interface to docs.getblock.io that AI agents can query directly:

```bash
https://docs.getblock.io/~gitbook/mcp
```

Instead of getting non-factual or hallucinated answers, your AI assistant connected to this server searches and reads our documentation from API references, guides, pricing, and setup steps etc, the moment you ask. The server uses streamable HTTP transport and requires no API key or authentication.

### What it can do

The server exposes four tools to any connected agent:

| Tool                  | What the agent does with it                                                         |
| --------------------- | ----------------------------------------------------------------------------------- |
| `searchDocumentation` | Searches all of docs.getblock.io and returns matching pages with excerpts and links |
| `getPage`             | Fetches the full markdown content of a specific docs page by URL                    |
| `askQuestion`         | Asks a question and gets an answer grounded in the documentation                    |
| `sendFeedback`        | Sends feedback about the documentation back to the GetBlock team                    |

Once connected, ask your assistant things like:

* “How do I enable the Yellowstone gRPC add-on on a dedicated Solana node?”
* “What are GetBlock's CU and rate limits per plan?”
* “Show me the Flashblocks API methods for Base.”

### Connect your AI agent

{% tabs %}
{% tab title="Claude" %}
In claude.ai or the Claude Desktop app:

1. Go to **Settings → Connectors → Add custom connector**.
2. Name it `GetBlock Docs` and paste `https://docs.getblock.io/~gitbook/mcp`.
3. Enable it from the tools menu in any chat.
{% endtab %}

{% tab title="Claude Code" %}
Run one command in your terminal:

```bash
claude mcp add --transport http getblock-docs https://docs.getblock.io/~gitbook/mcp
```

Verify the connection with `/mcp` inside a session.
{% endtab %}

{% tab title="Cursor" %}
Go to **Settings → MCP → Add new MCP server**, or add to `.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "getblock-docs": { "url": "https://docs.getblock.io/~gitbook/mcp" }
  }
}
```
{% endtab %}

{% tab title="VS Code" %}
Run **MCP: Add Server** from the Command Palette and choose HTTP, or add to `.vscode/mcp.json`:

{% code overflow="wrap" %}
```json
{
  "servers": {
    "getblock-docs": { "type": "http", "url": "https://docs.getblock.io/~gitbook/mcp" }
  }
}
```
{% endcode %}
{% endtab %}

{% tab title="ChatGPT" %}
1. Enable **Developer mode** under **Settings → Apps & Connectors**.
2. Choose **Create connector** and paste `https://docs.getblock.io/~gitbook/mcp` (no authentication).
3. The connector becomes available in chat and deep research.
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Migrating an existing project to GetBlock? Point your connected assistant at the [Migrate to GetBlock with AI](../migration/migrate-to-getblock-with-ai.md) guide and it will walk through the migration for you.
{% endhint %}
