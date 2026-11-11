---
description: >-
  Connect the GetBlock MCP server to Claude, Claude Code, Cursor, or VS Code and
  manage your GetBlock endpoints, tokens, and account from your AI assistant.
icon: mcp
---

# GetBlock MCP server

The GetBlock MCP server connects your AI assistant to your GetBlock account through the **Model Context Protocol (MCP)**. Instead of opening the dashboard, you ask your assistant to create an RPC endpoint, rotate a leaked token, or check your CU balance, and it makes the call for you:

```bash
https://mcp.getblock.io/v1
```

The server is built on the [GetBlock Platform API](../platform-api/overview.md) and uses the same API key for authentication. It acts only on the account to which the key belongs.

{% hint style="info" %}
This is a different server from the [GetBlock Docs MCP server](getblock-docs-mcp-server.md); it searches the documentation and requires no key. Connect both to give your assistant access to the docs and your account.
{% endhint %}

### What it can do

The server exposes 14 tools. Tools marked **Write** change your account.

<table data-search="false"><thead><tr><th>Area</th><th>Tool</th><th>What the agent does with it</th><th>Access</th></tr></thead><tbody><tr><td><strong>Catalog</strong></td><td><code>list_protocols</code></td><td>Lists the protocols available on Shared Nodes, filterable by name or by the chain a protocol is based on (e.g. <code>eth</code> for EVM chains)</td><td>Read</td></tr><tr><td></td><td><code>get_protocol</code></td><td>Returns one protocol's networks, sync modes, API types, regions, and add-ons: the valid values for creating a token</td><td>Read</td></tr><tr><td></td><td><code>list_addons</code></td><td>Lists the add-ons available on Shared Nodes and the protocols each one supports</td><td>Read</td></tr><tr><td><strong>Access tokens</strong></td><td><code>list_tokens</code></td><td>Lists your access tokens with their endpoint URLs</td><td>Read</td></tr><tr><td></td><td><code>create_token</code></td><td>Creates a Shared Node access token and returns its endpoint URL</td><td>Write</td></tr><tr><td></td><td><code>rotate_token</code></td><td>Replaces a token with a new one of the same configuration; the old endpoint URL stops working</td><td>Write</td></tr><tr><td></td><td><code>delete_token</code></td><td>Deletes a token; every client using its endpoint URL stops working</td><td>Write</td></tr><tr><td><strong>Dedicated Nodes</strong></td><td><code>list_dedicated_nodes</code></td><td>Lists your Dedicated Nodes and the APIs and add-ons each supports</td><td>Read</td></tr><tr><td></td><td><code>create_dedicated_token</code></td><td>Creates a token bound to one of your Dedicated Nodes</td><td>Write</td></tr><tr><td><strong>Limitless Nodes</strong></td><td><code>list_limitless_nodes</code></td><td>Lists your Limitless Nodes</td><td>Read</td></tr><tr><td></td><td><code>create_limitless_token</code></td><td>Creates a token bound to one of your Limitless Nodes</td><td>Write</td></tr><tr><td><strong>Account</strong></td><td><code>get_balance</code></td><td>Returns your CU balance and prepaid USD credits</td><td>Read</td></tr><tr><td></td><td><code>get_subscription</code></td><td>Returns your current plan and its status</td><td>Read</td></tr><tr><td></td><td><code>list_subscriptions</code></td><td>Lists every subscription on your account</td><td>Read</td></tr></tbody></table>

Once connected, ask your assistant things like:

* “Create a JSON-RPC endpoint for Ronin mainnet.”
* “Which regions can I use for peaq?”
* “Rotate my Solana token and show me the new endpoint URL.”
* “How many CUs do I have left, and which plan am I on?”

### Prerequisites

* A [GetBlock account](https://account.getblock.io/sign-in)
* A GetBlock API key. Create one in the dashboard under [**Settings → API Keys**](https://account.getblock.io/settings/api-keys); keys start with `gb_`.

```bash
export GETBLOCK_API_KEY="gb_..."
```

{% hint style="warning" %}
**The API key acts on your entire account.** An assistant holding it can delete tokens, which breaks every client using their endpoint URLs. Keep the key out of source control, and review write actions such as `delete_token` and `rotate_token` before you approve them.
{% endhint %}

### Connect your AI agent

Every request carries the header `Authorization: Bearer <API-KEY>`.

{% tabs %}
{% tab title="Claude" %}
In claude.ai or the Claude Desktop app:

1. Go to **Customize → Connectors**, click **+ Add**, and select **Add custom connector**.
2. Name it `GetBlock MCP Server`, paste `https://mcp.getblock.io/v1` as the URL, and click **Continue**.
3. Under **Authentication**, select **No sign-in**.
4. Under **Request headers**, choose `authorization` as the header and enter `<API-KEY>` as the value.&#x20;
5. Click **Add**, then enable the connector from the tools menu in any chat.

{% hint style="warning" %}
### NOTE THIS

1. The header value is stored securely and isn't shown again. To use a new key, edit the connector and enter the new value.
2. **Plan requirements:** Free plans can add one custom connector. On Team and Enterprise plans, an Owner adds the connector under **Organization settings → Connectors**, and each member then connects to it. See Anthropic’s guide to [custom connectors](https://support.claude.com/en/articles/11175166-get-started-with-custom-connectors-using-remote-mcp).
{% endhint %}
{% endtab %}

{% tab title="Claude Code" %}
Run one command in your terminal:

{% code overflow="wrap" %}
```bash
claude mcp add --transport http getblock https://mcp.getblock.io/v1 \
  --header "Authorization: Bearer $GETBLOCK_API_KEY"
```
{% endcode %}

Verify the connection with `/mcp` inside a session.
{% endtab %}

{% tab title="Cursor" %}
Go to **Settings → MCP → Add new MCP server**, or add to `.cursor/mcp.json`:

{% code overflow="wrap" %}
```json
{
  "mcpServers": {
    "getblock": {
      "url": "https://mcp.getblock.io/v1",
      "headers": { "Authorization": "Bearer <API-KEY>" }
    }
  }
}
```
{% endcode %}
{% endtab %}

{% tab title="VS Code" %}
Run **MCP: Add Server** from the Command Palette and choose HTTP, or add to `.vscode/mcp.json`:

{% code overflow="wrap" %}
```json
{
  "servers": {
    "getblock": {
      "type": "http",
      "url": "https://mcp.getblock.io/v1",
      "headers": { "Authorization": "Bearer <API-KEY>" }
    }
  }
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

### Good to know

* **Endpoint URLs are credentials.** The access token is embedded in the URL. Keep it in an environment variable and rotate it if it leaks.
* **Some actions stay in the dashboard.** Creating API keys, ordering Dedicated and Limitless Nodes, changing plans, and buying CU packages are done in the [GetBlock dashboard](https://account.getblock.io/).

{% hint style="info" %}
Migrating an existing project to GetBlock? Point your connected assistant at the [Migrate to GetBlock with AI](../migration/migrate-to-getblock-with-ai.md) guide and it will walk through the migration for you.
{% endhint %}
