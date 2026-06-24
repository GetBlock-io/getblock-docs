---
description: >-
  AI agent runbook to migrate your project's blockchain RPC endpoints to
  GetBlock using any coding agent.
---

# Migrate to Getblock with AI

This page is both a migration guide and an executable runbook for AI coding agents. Paste the one-liner below into any AI coding agent (Claude Code, Codex CLI, Cursor, Windsurf, Gemini CLI, Antigravity), and the agent fetches this page and follows it.

## Quick start

Paste this into your AI agent:

```bash
get docs.getblock.io/migrate-to-getblock-with-ai.md
```

The agent fetches this page and walks through the migration in three phases: **Assess**, **Plan**, and **Implement**. It stops after Plan and waits for your approval before making any changes to the code.

## How the agent works with GetBlock

GetBlock's documentation is published as machine-readable Markdown. The agent does not need a custom integration to read it.

* **Documentation index:** `https://docs.getblock.io/llms.txt` lists every doc page. The agent reads this first to discover what is available.
* **Full corpus in one file:** `https://docs.getblock.io/llms-full.txt` is the entire documentation concatenated, for agents that prefer a single fetch.
* **Any page as Markdown:** append `.md` to a doc URL, or fetch the page directly, to get clean Markdown instead of HTML.

{% hint style="warning" %}
Do not rely on training data for what GetBlock supports. Network coverage, methods, and interfaces change. Always verify against `llms.txt` and the live network reference pages. If something cannot be verified there, flag it as uncertain and point the user to GetBlock support rather than guessing.
{% endhint %}

The Assess and Plan phases run entirely on public docs and read-only checks. The Implement phase creates endpoints, which happens in the GetBlock dashboard. GetBlock endpoints embed your access token in the URL, so there is no API key to register with the agent and no auth header to manage.

{% hint style="info" %}
### Optional: on-chain reads during the dry run

If the agent needs to make live blockchain calls while testing (balances, transactions, block heights), GetBlock ships a local MCP server for ETH and Solana data queries: `github.com/GetBlock-io/mcp-server`. It runs locally with your access tokens as environment variables. This is optional. A plain `curl` against the new endpoint is enough to verify a migration.


{% endhint %}

## Agent instructions

Migrate this project's blockchain RPC endpoints to GetBlock.

### Assess

#### Coverage check

1. Scan the codebase for every blockchain RPC endpoint other than GetBlock. Check source files, environment variables, config files (`hardhat.config.*`, `foundry.toml`, `.env`, `truffle-config.js`), and any hardcoded URLs.
2. For each endpoint found, identify:

* The protocol and network (mainnet or a named testnet).
* The interface in use: JSON-RPC, REST, WebSocket, GraphQL, or gRPC.
* The methods called, and whether any need archive data (historical state, old blocks, `eth_getLogs` over wide ranges, trace or debug calls).
* Whether the project depends on streaming (WebSocket subscriptions, or Solana Yellowstone gRPC / Geyser).

3. Verify GetBlock coverage for each protocol. Fetch `https://docs.getblock.io/llms.txt`, find the network's reference page, and confirm the network, the interface, and the specific methods are documented. Fetch the page as Markdown for method-level detail.
4. Report four buckets: fully covered, covered but needs a specific setting (archive mode, MEV protection, a specific region), unsupported, and any endpoints in the codebase that are already dead or deprecated.
5. Flag these GetBlock features explicitly when the codebase needs them, so the user selects them at endpoint-creation time:

* **Archive mode.** A toggle when creating the endpoint. Required for historical state and full block-zero history. Available from the Starter plan up.
* **MEV protection.** A selectable interface that routes transactions through private channels to shield them from front-running and sandwich attacks. Off by default.
* **Region routing.** Endpoints resolve to a regional host: `go.getblock.io` (EU, Frankfurt), `go.getblock.us` (US, New York), `go.getblock.asia` (Asia, Singapore). Latency-sensitive workloads should pick the nearest region.
* **WebSocket.** Same token, `wss://` scheme: `wss://go.getblock.io/<ACCESS_TOKEN>/`.
* **Yellowstone gRPC / Geyser** for high-throughput Solana streaming. Confirm availability on the Solana reference page.
* **REST and GraphQL** interfaces where the protocol supports them (for example, TON REST, TRON HTTP API).

#### Cost and savings estimate

If the user is migrating from another RPC provider, estimate the GetBlock cost and the savings versus their current bill. GetBlock meters usage in Compute Units (CUs).

1. Get the user's current usage from their existing provider, lowest-friction path first:

* **QuickNode** exposes usage through its Admin API. Ask the user for a QuickNode API key and fetch their billing-period usage and per-method breakdown.
* **Alchemy** has no usage API. Ask for a screenshot of Settings, then Usage, and Settings, then Billing. Read the Compute Units used, plan, and spend straight from the image.
* **Any other provider** or no programmatic access: ask for a screenshot of the usage and billing dashboard, or the approximate monthly request volume and current spend.

2. Pull GetBlock's live plans and the CU model from `https://getblock.io/pricing` and the Plans and limits doc page.&#x20;

{% hint style="warning" %}
Do not quote prices from memory; they change. Fit the user's converted usage to the cheapest plan that covers it, and present the monthly GetBlock cost next to their current bill.
{% endhint %}

3. Route results based on volume: high-volume, dedicated-node needs, or enterprise terms go to GetBlock sales; self-serve volumes go straight to the dashboard.

#### Dry run

If the user already has GetBlock endpoints, or is willing to create one test endpoint first:

1. For each method the codebase uses, send a test request to the matching GetBlock endpoint and confirm the response shape matches what the app expects.
2. If the codebase has known throughput patterns (batch calls, polling intervals, concurrent requests), test at that rate and watch for rate-limit responses.
3. Report what works, what fails, and any method or rate-limit gaps.

### Plan

For each endpoint to migrate, lay out the exact replacement: which GetBlock network, interface, region, and mode (full or archive, MEV on or off) it maps to, and whether a new endpoint needs to be created. Flag anything that needs the user's decision (region choice, archive cost, unsupported chains that need a dedicated node or a sales conversation).

{% hint style="warning" %}
**STOP here.** Present the plan and wait for the user to approve or adjust before changing anything. Do not edit code until the user confirms. Answer follow-up questions using the docs. For unsupported chains, point the user to GetBlock support for information on dedicated-node availability.
{% endhint %}

### Implement

GetBlock endpoints are created in the dashboard, not via a remote API, so this phase involves a guided handoff and code changes.

1. Tell the user exactly which endpoints to create at `https://account.getblock.io` (or the GetBlock dashboard), with the precise settings from the approved plan:&#x20;

* protocol
* network
* interface
* region,&#x20;
* archive on/off,&#x20;
* MEV on/off.&#x20;

Each created endpoint produces a URL of the form `https://go.getblock.io/<ACCESS_TOKEN>/`.

2. Have the user paste the new endpoint URLs.&#x20;

{% hint style="warning" %}
The token is the credential, so never ask the user to paste it into chat in plaintext; instead, reference it from the `.env` var.
{% endhint %}

3. Replace each old endpoint with its GetBlock equivalent across source, env files, and build configs.
4. Verify each swap with a live call before declaring it done (see snippets below).
5. Summarize what changed: files touched, endpoints swapped, and anything still pending (a dedicated node, a sales follow-up, an unverified method).

## Feature mapping

| Need in the codebase             | GetBlock equivalent                                           |
| -------------------------------- | ------------------------------------------------------------- |
| HTTP JSON-RPC                    | `https://go.getblock.io/<ACCESS_TOKEN>/`                      |
| WebSocket subscriptions          | `wss://go.getblock.io/<ACCESS_TOKEN>/`                        |
| Historical / archive queries     | Archive mode toggle at endpoint creation (Starter plan+)      |
| Front-running protection         | MEV Protected interface                                       |
| Region pinning for latency       | `.io` (Frankfurt), `.us` (New York), `.asia` (Singapore) host |
| Solana high-throughput streaming | Yellowstone gRPC / Geyser                                     |
| REST or GraphQL interface        | Select the matching interface where the protocol offers it    |
| Many chains in one place         | One account, one token format, 130+ networks                  |

## Verification snippets

1. EVM chain ID check (confirms the endpoint is live and on the expected network):

```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS_TOKEN>/' \
  --header 'Content-Type: application/json' \
  --data-raw '{"jsonrpc":"2.0","method":"eth_chainId","params":[],"id":"getblock.io"}'
```

2. Latest block height:

```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS_TOKEN>/' \
  --header 'Content-Type: application/json' \
  --data-raw '{"jsonrpc":"2.0","method":"eth_blockNumber","params":[],"id":"getblock.io"}'
```

3. WebSocket smoke test:

```bash
wscat -c 'wss://go.getblock.io/<ACCESS_TOKEN>/'
# then send:
{"jsonrpc":"2.0","method":"eth_blockNumber","params":[],"id":"getblock.io"}
```

4. Library swap examples the agent can apply directly:

{% tabs %}
{% tab title="ethers" %}
{% code overflow="wrap" %}
```js
// ethers v6
const provider = new ethers.JsonRpcProvider("https://go.getblock.io/<ACCESS_TOKEN>/");
```
{% endcode %}
{% endtab %}

{% tab title="viem" %}
```js
// viem
const client = createPublicClient({
  chain: mainnet,
  transport: http("https://go.getblock.io/<ACCESS_TOKEN>/"),
});
```
{% endtab %}

{% tab title="web3.py" %}
```python
# web3.py
w3 = Web3(Web3.HTTPProvider("https://go.getblock.io/<ACCESS_TOKEN>/"))
```
{% endtab %}
{% endtabs %}

## Support

For implementation issues, unsupported chains, or dedicated-node and enterprise questions, point the user to [GetBlock support](mailto:support@getblock.io). Keep the token secret at every step: it is the full credential, embedded in the URL.
