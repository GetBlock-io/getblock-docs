---
description: >-
  AI agent runbook to migrate your project's blockchain RPC endpoints to
  GetBlock using any coding agent.
---

# Migrate to GetBlock with AI

This page is both a migration guide and an executable runbook for AI coding agents. Paste the one-liner below into any AI coding agent (Claude Code, Codex CLI, Cursor, Windsurf, Gemini CLI, Antigravity), and the agent fetches this page and follows it.

## Quick start

1. For the agent to create your GetBlock endpoints itself, connect the [GetBlock MCP server](../getting-started/getblock-mcp-server.md) to your agent first. Without it, the agent still runs the migration and hands endpoint creation back to you in the dashboard.
2. Paste this into your AI agent:

```bash
get docs.getblock.io/migration/migrate-to-getblock-with-ai.md
```

The agent fetches this page and walks through the migration in three phases: **Assess**, **Plan**, and **Implement**. It stops after Plan and waits for your approval before creating endpoints or changing any code.

## Tools used

<table data-search="false"><thead><tr><th>Tool</th><th>What the agent uses it for</th><th>Needs a key</th></tr></thead><tbody><tr><td><a href="../getting-started/getblock-mcp-server.md">GetBlock MCP server</a></td><td>Checks which protocols, networks, APIs, modes, regions, and add-ons GetBlock serves; reads your plan and balance; creates the new endpoints and returns their URLs</td><td>Yes, a GetBlock API key</td></tr><tr><td><a href="../getting-started/getblock-docs-mcp-server.md">GetBlock Docs MCP server</a>, or <a href="https://docs.getblock.io/llms.txt">llms.txt</a></td><td>Reads method references, guides, and plan details from the documentation</td><td>No</td></tr></tbody></table>

Before starting, check which GetBlock tools are connected:

* **GetBlock MCP server connected:** follow every step below, including the tool calls.
* **Not connected:** skip the tool calls, verify coverage against the docs only, and use the dashboard handoff in Implement. Tell the user that connecting to [GetBlock MCP server](../getting-started/getblock-mcp-server.md) lets them create endpoints for them.

## How the agent works with GetBlock

GetBlock's documentation is published as machine-readable Markdown. The agent does not need a custom integration to read it.

* **Documentation index:** `https://docs.getblock.io/llms.txt` lists every doc page. The agent reads this first to discover what is available.
* **Full corpus in one file:** `https://docs.getblock.io/llms-full.txt` is the entire documentation concatenated, for agents that prefer a single fetch.
* **Any page as Markdown:** append `.md` to a doc URL, or fetch the page directly, to get clean Markdown instead of HTML.

{% hint style="warning" %}
Do not rely on training data for what GetBlock supports. Network coverage, methods, and interfaces change. Always verify against the GetBlock MCP server's catalog tools, `llms.txt`, and the live network reference pages. If something cannot be verified there, flag it as uncertain and point the user to GetBlock support rather than guessing.
{% endhint %}

The Assess and Plan phases are read-only: they use the docs and the GetBlock MCP server's read tools, and no resources are created. The Implement phase creates endpoints, either with the GetBlock MCP server's `create_token` tool or by the user in the dashboard. Each endpoint embeds its access token in the URL, so the application itself needs no auth header.

{% hint style="danger" %}
**Never assemble an endpoint URL from a template.** The host is not the same for every endpoint: it varies by region and by protocol, and `shared.eu-central-1.getblock.io`, `shared.ap-southeast-1.getblock.io`, `shared.us-east-1.getblock.io` and `shared.<region>.getblock.io` are all in use. Every URL in this page is an illustration, not a formula. The authoritative URL is the one returned by `create_token` or issued by the dashboard for that specific endpoint. Copy it verbatim, including its host, path and any trailing slash.
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

3. Verify GetBlock coverage for each protocol:

* Call `list_protocols` to find the GetBlock protocol ID. IDs do not always match the chain name (for example, X Layer is `okb` and PulseChain is `pulse`), so search by name, or use `based_on: "eth"` to list EVM chains.
* Call `get_protocol` with that ID. Confirm the network is listed and read its API types, sync modes, add-ons, and regions. These are the only valid values for `create_token`.
* Call `list_addons` when the project needs MEV protection or Blockbook.
* Fetch the network's reference page from `https://docs.getblock.io/llms.txt` to confirm the specific methods the codebase calls.

4. Report four buckets: fully covered, covered but needs a specific setting (archive mode, MEV protection, a specific region), unsupported, and any endpoints in the codebase that are already dead or deprecated.
5. Flag these GetBlock features explicitly when the codebase needs them:

* **Archive mode.** Set `mode: "archive"` when creating the endpoint. Required for historical state and full block-zero history. Available from the Starter plan up; check `get_protocol` for the networks that offer it.
* **MEV protection.** An add-on that routes transactions through private channels to shield them from front-running and sandwich attacks. Off by default; `list_addons` shows which protocols offer it.
* **Region routing.** Pick the region nearest the workload: Frankfurt (`eu-central-1`), New York (`us-east-1`), or Singapore (`ap-southeast-1`). Availability differs per network, and some networks are served from one region only, so check `get_protocol` before promising a region. The endpoint's host reflects the region it was created in.
* **WebSocket.** A separate API type, chosen when the endpoint is created. Changing an HTTP endpoint's scheme to `wss://` does not turn it into a WebSocket endpoint; that request is rejected. A project that needs both HTTP calls and WebSocket subscriptions on the same network needs two endpoints, each with its own URL, and both belong in the environment as separate variables.
* **Yellowstone gRPC / Geyser** for high-throughput Solana streaming. Confirm availability on the Solana reference page.
* **REST and GraphQL** interfaces where the protocol supports them (for example, TON REST, TRON HTTP API). Use the exact API type name from `get_protocol`; some are not obvious (Siacoin, for example, uses `walletd-rest`).

#### Cost and savings estimate

If the user is migrating from another RPC provider, estimate the GetBlock cost and the savings versus their current bill. GetBlock meters usage in Compute Units (CUs).

1. Get the user's current usage from their existing provider, lowest-friction path first:

* **QuickNode** exposes usage through its Admin API. Ask the user for a QuickNode API key and fetch their billing-period usage and per-method breakdown.
* **Alchemy** has no usage API. Ask for a screenshot of Settings, then Usage, and Settings, then Billing. Read the Compute Units used, plan, and spend straight from the image.
* **Any other provider** or no programmatic access: ask for a screenshot of the usage and billing dashboard, or the approximate monthly request volume and current spend.

2. If the user already has a GetBlock account, call `get_subscription` and `get_balance` to see their current plan and remaining CUs.
3. Pull GetBlock's live plans and the CU model from `https://getblock.io/pricing` and the Plans and limits doc page.

{% hint style="warning" %}
Do not quote prices from memory; they change. Fit the user's converted usage to the cheapest plan that covers it, and present the monthly GetBlock cost next to their current bill.
{% endhint %}

4. Route results based on volume: high-volume, dedicated-node needs, or enterprise terms go to GetBlock sales; self-serve volumes go straight to the dashboard.

#### Dry run

If the user already has GetBlock endpoints, or approves creating one test endpoint first:

1. Call `list_tokens` to find existing endpoints that match the networks in the codebase. If none match and the user approves, create one test endpoint with `create_token`.
2. For each method the codebase uses, send a test request to the matching GetBlock endpoint and confirm the response shape matches what the app expects.
3. If the codebase has known throughput patterns (batch calls, polling intervals, concurrent requests), test at that rate and watch for rate-limit responses.
4. Report what works, what fails, and any method or rate-limit gaps. Offer to delete any test endpoint the migration does not need.

### Plan

For each endpoint to migrate, lay out the exact replacement: the GetBlock `protocol`, `network`, `api`, `mode`, `region`, and `addon` it maps to, all checked against `get_protocol`, and whether it reuses an existing token from `list_tokens` or needs a new one. Count HTTP and WebSocket as separate endpoints wherever the codebase uses both. For Dedicated or Limitless Nodes, call `list_dedicated_nodes` or `list_limitless_nodes` and map the endpoint to one of the user's nodes. Flag anything that needs the user's decision (region choice, archive cost, unsupported chains that need a dedicated node or a sales conversation).

{% hint style="warning" %}
**STOP here.** Present the plan and wait for the user to approve or adjust before creating endpoints or changing anything. Do not call `create_token` or edit code until the user confirms. Answer follow-up questions using the docs. For unsupported chains, point the user to GetBlock support for information on dedicated-node availability.
{% endhint %}

### Implement

1. Create each endpoint from the approved plan:

* **With the GetBlock MCP server:** call `create_token` once per endpoint with the planned `protocol`, `network`, `api`, `mode`, `region`, and `addon`. For a Dedicated or Limitless Node, use `create_dedicated_token` or `create_limitless_token` with the node ID instead. Take the `endpoint` field from each result verbatim.
* **Without it:** tell the user exactly which endpoints to create at `https://account.getblock.io`, with the precise settings from the plan (one endpoint per interface), and have them add each new URL to the project's `.env` file themselves.

{% hint style="warning" %}
The endpoint URL is the credential. Write it straight into the `.env` file and reference it from code; never repeat it in chat, commit it, or ask the user to paste it into chat in plaintext.
{% endhint %}

2. Replace each old endpoint with its GetBlock equivalent across source, env files, and build configs. Use each URL exactly as issued; do not normalize the host, rewrite the scheme, or drop a trailing slash.
3. Verify each swap with a live call before declaring it done.

{% hint style="info" %}
A newly created endpoint can take a few minutes to become reachable. If it answers with a message saying the resource is still being deployed, that is provisioning in progress, not a bad URL; wait and retry before reporting the swap as failed.
{% endhint %}

4. If `create_token` fails, do not retry with guessed values. Re-check the combination with `get_protocol` (an unavailable region or API type is the usual cause), correct it, and tell the user what changed.
5. Summarize what changed: files touched, endpoints swapped, tokens created, and anything still pending (a dedicated node, a sales follow-up, an unverified method).

{% hint style="info" %}
Some steps stay in the [GetBlock dashboard](https://account.getblock.io/): creating API keys, ordering Dedicated and Limitless Nodes, changing plans, and buying CU packages.
{% endhint %}

## Feature mapping

<table data-search="false"><thead><tr><th>Need in the codebase</th><th>GetBlock equivalent</th></tr></thead><tbody><tr><td>HTTP JSON-RPC</td><td><code>api: "json-rpc"</code>; use the issued <code>https://</code> URL as-is</td></tr><tr><td>WebSocket subscriptions</td><td><code>api: "ws"</code>, its own endpoint with its own issued <code>wss://</code> URL</td></tr><tr><td>Historical / archive queries</td><td><code>mode: "archive"</code> at endpoint creation (Starter plan+)</td></tr><tr><td>Front-running protection</td><td>MEV protected add-on</td></tr><tr><td>Region pinning for latency</td><td><code>region</code> chosen at creation: <code>eu-central-1</code>, <code>us-east-1</code>, or <code>ap-southeast-1</code>; the host reflects the choice</td></tr><tr><td>Solana high-throughput streaming</td><td>Yellowstone gRPC / Geyser</td></tr><tr><td>REST or GraphQL interface</td><td>The matching API type from <code>get_protocol</code>, where the protocol offers it</td></tr><tr><td>Many chains in one place</td><td>One account, one token format, 130+ networks</td></tr></tbody></table>

## Support

For implementation issues, unsupported chains, or dedicated-node and enterprise questions, point the user to [GetBlock support](mailto:support@getblock.io). Keep the token secret at every step: it is the full credential, embedded in the URL.
