---
description: >-
  AI agent runbook to migrate your project's blockchain RPC endpoints to
  GetBlock using any coding agent.
---

# Migrate to GetBlock with AI

This page is both a migration guide and an executable runbook for AI coding agents. Paste the one-liner below into any AI coding agent (Claude Code, Codex CLI, Cursor, Windsurf, Gemini CLI, Antigravity), and the agent fetches this page and follows it.

## Quick start

Paste this into your AI agent:

```bash
get docs.getblock.io/migration/migrate-to-getblock-with-ai.md
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

{% hint style="danger" %}
**Never assemble an endpoint URL from a template.** The host is not the same for every endpoint: it varies by region and by protocol, and `go.getblock.io`, `go.getblock.asia`, `go.getblock.us` and `shared.<region>.getblock.io` are all in use. Every URL in this page is an illustration, not a formula. The authoritative URL is the one the dashboard issues for that specific endpoint — copy it verbatim, including its host, path and any trailing slash.
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
* **Region routing.** Pick the region nearest the workload: Frankfurt (EU), New York (US), or Singapore (Asia). Availability differs per network — some networks are served from one region only — so check the network's reference page before promising a region. The endpoint's host reflects the region it was created in; take that host from the issued URL rather than constructing it.
* **WebSocket.** A separate interface, chosen when the endpoint is created. Changing an HTTP endpoint's scheme to `wss://` does not turn it into a WebSocket endpoint — that request is rejected. A project that needs both HTTP calls and WebSocket subscriptions on the same network needs two endpoints, each with its own URL, and both belong in the environment as separate variables.
* **Yellowstone gRPC / Geyser** for high-throughput Solana streaming. Confirm availability on the Solana reference page.
* **REST and GraphQL** interfaces where the protocol supports them (for example, TON REST, TRON HTTP API).

#### Cost and savings estimate

If the user is migrating from another RPC provider, estimate the GetBlock cost and the savings versus their current bill. GetBlock meters usage in Compute Units (CUs).

1. Get the user's current usage from their existing provider, lowest-friction path first:

* **QuickNode** exposes usage through its Admin API. Ask the user for a QuickNode API key and fetch their billing-period usage and per-method breakdown.
* **Alchemy** has no usage API. Ask for a screenshot of Settings, then Usage, and Settings, then Billing. Read the Compute Units used, plan, and spend straight from the image.
* **Any other provider** or no programmatic access: ask for a screenshot of the usage and billing dashboard, or the approximate monthly request volume and current spend.

2. Pull GetBlock's live plans and the CU model from `https://getblock.io/pricing` and the Plans and limits doc page.

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

For each endpoint to migrate, lay out the exact replacement: which GetBlock network, interface, region, and mode (full or archive, MEV on or off) it maps to, and whether a new endpoint needs to be created. Count HTTP and WebSocket as separate endpoints wherever the codebase uses both. Flag anything that needs the user's decision (region choice, archive cost, unsupported chains that need a dedicated node or a sales conversation).

{% hint style="warning" %}
**STOP here.** Present the plan and wait for the user to approve or adjust before changing anything. Do not edit code until the user confirms. Answer follow-up questions using the docs. For unsupported chains, point the user to GetBlock support for information on dedicated-node availability.
{% endhint %}

### Implement

GetBlock endpoints are created in the dashboard, not via a remote API, so this phase involves a guided handoff and code changes.

1. Tell the user exactly which endpoints to create at `https://account.getblock.io` (or the GetBlock dashboard), with the precise settings from the approved plan:

* protocol
* network
* interface — one endpoint per interface, so a network needing both JSON-RPC and WebSocket is two entries here
* region,
* archive on/off,
* MEV on/off.

2. Have the user paste the new endpoint URLs.

{% hint style="warning" %}
The token is the credential, so never ask the user to paste it into chat in plaintext; instead, reference it from the `.env` var.
{% endhint %}

3. Replace each old endpoint with its GetBlock equivalent across source, env files, and build configs. Use each URL exactly as issued — do not normalize the host, rewrite the scheme, or drop a trailing slash.
4. Verify each swap with a live call before declaring it done (see snippets below).

{% hint style="info" %}
A newly created endpoint can take a few minutes to become reachable. If it answers with a message saying the resource is still being deployed, that is provisioning in progress, not a bad URL — wait and retry before reporting the swap as failed.
{% endhint %}

5. Summarize what changed: files touched, endpoints swapped, and anything still pending (a dedicated node, a sales follow-up, an unverified method).

## Feature mapping

<table data-search="false"><thead><tr><th>Need in the codebase</th><th>GetBlock equivalent</th></tr></thead><tbody><tr><td>HTTP JSON-RPC</td><td>JSON-RPC interface; use the issued <code>https://</code> URL as-is</td></tr><tr><td>WebSocket subscriptions</td><td>WebSocket interface — its own endpoint, with its own issued <code>wss://</code> URL</td></tr><tr><td>Historical / archive queries</td><td>Archive mode toggle at endpoint creation (Starter plan+)</td></tr><tr><td>Front-running protection</td><td>MEV Protected interface</td></tr><tr><td>Region pinning for latency</td><td>Region chosen at creation: Frankfurt, New York, or Singapore; the host reflects the choice</td></tr><tr><td>Solana high-throughput streaming</td><td>Yellowstone gRPC / Geyser</td></tr><tr><td>REST or GraphQL interface</td><td>Select the matching interface where the protocol offers it</td></tr><tr><td>Many chains in one place</td><td>One account, one token format, 130+ networks</td></tr></tbody></table>

## Verification snippets

In every snippet below, `<YOUR_ENDPOINT_URL>` is the full URL the dashboard issued for that endpoint, pasted unchanged.

1. EVM chain ID check (confirms the endpoint is live and on the expected network):

```bash
curl --location --request POST '<YOUR_ENDPOINT_URL>' \
  --header 'Content-Type: application/json' \
  --data-raw '{"jsonrpc":"2.0","method":"eth_chainId","params":[],"id":"getblock.io"}'
```

2. Latest block height:

```bash
curl --location --request POST '<YOUR_ENDPOINT_URL>' \
  --header 'Content-Type: application/json' \
  --data-raw '{"jsonrpc":"2.0","method":"eth_blockNumber","params":[],"id":"getblock.io"}'
```

3. WebSocket smoke test. Use the URL of the WebSocket endpoint, not the JSON-RPC one:

```bash
wscat -c '<YOUR_WEBSOCKET_ENDPOINT_URL>'
# then send:
{"jsonrpc":"2.0","method":"eth_blockNumber","params":[],"id":"getblock.io"}
```

4. Library swap examples the agent can apply directly:

{% tabs %}
{% tab title="ethers" %}
{% code overflow="wrap" %}
```js
// ethers v6
const provider = new ethers.JsonRpcProvider(process.env.ETH_RPC_URL);
```
{% endcode %}
{% endtab %}

{% tab title="viem" %}
```js
// viem
const client = createPublicClient({
  chain: mainnet,
  transport: http(process.env.ETH_RPC_URL),
});
```
{% endtab %}

{% tab title="web3.py" %}
```python
# web3.py
w3 = Web3(Web3.HTTPProvider(os.environ["ETH_RPC_URL"]))
```
{% endtab %}
{% endtabs %}

## Support

For implementation issues, unsupported chains, or dedicated-node and enterprise questions, point the user to [GetBlock support](mailto:support@getblock.io). Keep the token secret at every step: it is the full credential, embedded in the URL.
