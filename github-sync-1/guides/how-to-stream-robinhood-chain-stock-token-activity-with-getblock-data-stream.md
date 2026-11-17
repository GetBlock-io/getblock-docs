---
description: >-
  Build a live feed of tokenized stock mints, burns, and transfers on
  Robinhood   Chain using the erc20Transfers topic of GetBlock EVM Data Stream.
---

# How to Stream Robinhood Chain Stock Token Activity With GetBlock Data Stream

Robinhood Chain is an Arbitrum-based Ethereum L2 built for tokenized stocks. Every share of TSLA, NVDA, or AAPL on it is an ERC-20 Stock Token that moves 24/7, including when the stock exchanges are closed. Robinhood Chain also produces a block about every 100ms, so polling RPC for `Transfer` logs means sending a large number of `eth_getLogs` calls just to keep up.&#x20;

Each poll can also miss logs at the edges of a block range, and most calls return no new results. Wallets, portfolio trackers, and compliance dashboards need each mint, burn, and transfer as soon as it occurs, without having to run their own indexer.

In this guide, you will learn how to stream live Stock Token activity on Robinhood Chain using the `erc20Transfers` topic of GetBlock EVM Data Stream.

## What you'll build

A `watchStockTokens(watchlist, options)` function that:

1. Opens one authenticated WebSocket connection to the Robinhood Chain stream.
2. Subscribes to `erc20Transfers` once per Stock Token in your watchlist.
3. Classifies each event as a **MINT** (shares issued), **BURN** (shares redeemed), or **TRANSFER**.
4. Converts the raw 18-decimal amount to shares and skips movements below your size threshold.
5. Drops duplicate deliveries, flags reorg corrections, and reconnects automatically if the connection drops.

## Prerequisites

* **Node.js 18+**: the project uses ES modules and `BigInt`.
* A [**GetBlock account**](https://account.getblock.io/) with CU available. Data Stream is billed at 10 CU per delivered event.
* A **GetBlock API key**: create one under [**Settings → API Keys**](https://account.getblock.io/settings/api-keys). Keys start with `gb_`.
* Basic JavaScript knowledge.

## Project Setup

{% stepper %}
{% step %}
### Create the project and install dependencies

Create a Node project and install `ws` for the WebSocket client and `dotenv` for configuration:

```bash
mkdir robinhood-stock-token-stream && cd robinhood-stock-token-stream
npm init -y
npm install ws dotenv
```

Set `"type": "module"` and a start script in `package.json`:

{% code title="package.json" %}
```json
{
  "name": "robinhood-stock-token-stream",
  "type": "module",
  "scripts": {
    "start": "node index.js"
  }
}
```
{% endcode %}
{% endstep %}

{% step %}
### Configure your API key

Store your key and an optional minimum alert size in a `.env` file:

{% code title=".env" %}
```bash
GETBLOCK_API_KEY=gb_your_api_key
# Only alert on movements of at least this many shares (0 = everything)
MIN_SHARES=0
```
{% endcode %}

{% hint style="warning" %}
Data Stream authenticates with the `gb_` API key in an `Authorization: Bearer` header. Your Robinhood Chain RPC access token does not work here. Never commit `.env`; add it to `.gitignore`.
{% endhint %}
{% endstep %}

{% step %}
### Define the Stock Token watchlist

Each Robinhood Stock Token is a standard ERC-20 with 18 decimals. `tokenContract` takes a single address, so the app opens one subscription per token. Keep the list at 15 tokens or fewer, because each account can have at most 15 subscriptions.

{% code title="tokens.js" overflow="wrap" %}
```js
// Robinhood Chain mainnet (chain ID 4663) Stock Tokens.
// Each one is a standard ERC-20 with 18 decimals. Add or remove entries to change
// your watchlist. Each entry opens one subscription, and the limit is 15 per account.
export const STOCK_TOKENS = [
  { symbol: "TSLA", address: "0x322F0929c4625eD5bAd873c95208D54E1c003b2d", decimals: 18 },
  { symbol: "NVDA", address: "0xd0601CE157Db5bdC3162BbaC2a2C8aF5320D9EEC", decimals: 18 },
  { symbol: "AAPL", address: "0xaF3D76f1834A1d425780943C99Ea8A608f8a93f9", decimals: 18 },
  { symbol: "MSFT", address: "0xe93237C50D904957Cf27E7B1133b510C669c2e74", decimals: 18 },
  { symbol: "AMZN", address: "0x12f190a9F9d7D37a250758b26824B97CE941bF54", decimals: 18 },
  { symbol: "GOOGL", address: "0x2e0847E8910a9732eB3fb1bb4b70a580ADAD4FE3", decimals: 18 },
  { symbol: "META", address: "0xc0D6457C16Cc70d6790Dd43521C899C87ce02f35", decimals: 18 },
  { symbol: "SPY", address: "0x117cc2133c37B721F49dE2A7a74833232B3B4C0C", decimals: 18 },
];
```
{% endcode %}
{% endstep %}

{% step %}
### Build the stream client

`stream.js` contains everything specific to Data Stream. It authenticates, sends the subscribe requests, and routes replies and events. It also drops duplicates and reconnects when the connection closes:

{% code title="stream.js" overflow="wrap" %}
```js
import WebSocket from "ws";

const MAX_BACKOFF_MS = 30_000;
const MAX_TRACKED_EVENTS = 50_000;

/**
 * Opens a GetBlock EVM Data Stream connection, subscribes to every request in
 * `subscriptions`, and calls `onEvent(event, subscription)` once per new event.
 * Reconnects with exponential backoff and resubscribes after any disconnect.
 */
export function createStream({ url, apiKey, subscriptions, onEvent, onStatus = console.log }) {
  const revisions = new Map(); // eventId -> greatest revision applied
  let ws;
  let attempt = 0;
  let closedByUser = false;

  function connect() {
    ws = new WebSocket(url, { headers: { Authorization: `Bearer ${apiKey}` } });
    const pending = new Map(); // request id -> subscription
    const active = new Map(); // subscription id -> subscription

    ws.on("open", () => {
      onStatus("Connected. Subscribing...");
      subscriptions.forEach((sub, i) => {
        const id = i + 1;
        pending.set(id, sub);
        ws.send(JSON.stringify({
          jsonrpc: "2.0",
          id,
          method: "getblock_subscribe",
          params: [{ topic: sub.topic, filters: sub.filters }],
        }));
      });
    });

    ws.on("message", (raw) => {
      const msg = JSON.parse(raw);

      // Replies to our subscribe requests carry the request id
      if (msg.id !== undefined) {
        const sub = pending.get(msg.id);
        pending.delete(msg.id);
        if (msg.error) {
          onStatus(`Subscribe failed for ${sub?.label}: ${msg.error.code} ${msg.error.message}`);
          return;
        }
        active.set(msg.result, sub);
        attempt = 0; // a working subscription resets the backoff
        onStatus(`Subscribed to ${sub.label} (${msg.result})`);
        return;
      }

      // Everything else is an event notification
      const { subscription, result: event } = msg.params;
      if (!isNewer(event)) return; // duplicate or stale revision
      onEvent(event, active.get(subscription));
    });

    ws.on("close", (code, reason) => {
      if (closedByUser) return;
      const delay = Math.min(1000 * 2 ** attempt, MAX_BACKOFF_MS);
      attempt += 1;
      const hint = code === 1006 ? " (handshake rejected or network drop: check the API key)" : "";
      onStatus(`Disconnected (${code}${reason.length ? ` ${reason}` : ""})${hint}. Reconnecting in ${delay / 1000}s...`);
      setTimeout(connect, delay);
    });

    ws.on("error", (err) => onStatus(`Socket error: ${err.message}`));
  }

  // Delivery is at-least-once: keep only the greatest revision per eventId
  function isNewer({ eventId, revision }) {
    const previous = revisions.get(eventId);
    if (previous !== undefined && revision <= previous) return false;
    revisions.delete(eventId);
    revisions.set(eventId, revision);
    if (revisions.size > MAX_TRACKED_EVENTS) {
      revisions.delete(revisions.keys().next().value); // forget the oldest
    }
    return true;
  }

  connect();

  return {
    close() {
      closedByUser = true;
      ws.close();
    },
  };
}
```
{% endcode %}

Three details make this client production-safe:

* **Deduplication by `eventId` and `revision`.** Delivery is at-least-once, so the same event can arrive twice. Keeping the greatest `revision` for each `eventId` makes duplicates harmless.
* **Resubscribe on reconnect.** Subscriptions belong to a single socket and end when it closes. The client sends every `getblock_subscribe` again each time it reconnects.
* **No client-side pings.** The server pings about every 30 seconds, and `ws` answers automatically, so an idle connection stays open without extra code.
{% endstep %}

{% step %}
### Build the Stock Token watcher

`stocks.js` turns raw `erc20Transfers` events into Stock Token alerts. Robinhood mints new Stock Tokens from the zero address and burns redeemed ones by sending them to it, so checking `from` and `to` is enough to tell mints and burns apart from ordinary transfers:

{% code title="stocks.js" overflow="wrap" %}
```js
import { createStream } from "./stream.js";

const STREAM_URL = "wss://stream.eu-central-1.getblock.io/v1/robinhood-mainnet/stream";
const ZERO_ADDRESS = "0x0000000000000000000000000000000000000000";
const MAX_SUBSCRIPTIONS = 15;

/**
 * Streams live Stock Token movements on Robinhood Chain and calls `onAlert`
 * for every mint, burn, or transfer of at least `minShares` shares.
 */
export function watchStockTokens(watchlist, { apiKey, minShares = "0", onAlert, onStatus, url = STREAM_URL }) {
  if (!apiKey) throw new Error("GETBLOCK_API_KEY is not set. Copy .env.example to .env and add your key.");
  if (watchlist.length > MAX_SUBSCRIPTIONS) {
    throw new Error(`Watchlist has ${watchlist.length} tokens; the limit is ${MAX_SUBSCRIPTIONS} subscriptions per account.`);
  }

  const byAddress = new Map(
    watchlist.map((t) => [t.address.toLowerCase(), { ...t, minRaw: parseUnits(minShares, t.decimals) }])
  );

  // One erc20Transfers subscription per token: tokenContract takes a single address
  const subscriptions = watchlist.map((t) => ({
    label: t.symbol,
    topic: "erc20Transfers",
    filters: { tokenContract: t.address },
  }));

  return createStream({
    url,
    apiKey,
    subscriptions,
    onStatus,
    onEvent(event) {
      const { data, removed } = event;
      const token = byAddress.get(data.tokenContract.toLowerCase());
      if (!token) return;

      const raw = BigInt(data.value);
      if (raw < token.minRaw) return;

      onAlert({
        type: removed ? "REVERTED" : classify(data),
        symbol: token.symbol,
        shares: formatUnits(raw, token.decimals),
        from: data.from,
        to: data.to,
        block: Number(BigInt(data.blockNumber)),
        time: new Date(Number(BigInt(data.blockTimestamp)) * 1000).toISOString(),
        txHash: data.transactionHash,
        explorer: `https://robinhoodchain.blockscout.com/tx/${data.transactionHash}`,
        eventId: event.eventId,
      });
    },
  });
}

// Stock Tokens are minted from and burned to the zero address
export function classify({ from, to }) {
  if (from.toLowerCase() === ZERO_ADDRESS) return "MINT";
  if (to.toLowerCase() === ZERO_ADDRESS) return "BURN";
  return "TRANSFER";
}

// BigInt-safe conversions: 18-decimal values overflow JavaScript numbers
export function formatUnits(raw, decimals) {
  const base = 10n ** BigInt(decimals);
  const whole = raw / base;
  const fraction = (raw % base).toString().padStart(decimals, "0").replace(/0+$/, "");
  return fraction ? `${whole}.${fraction}` : `${whole}`;
}

export function parseUnits(value, decimals) {
  const [whole, fraction = ""] = String(value).trim().split(".");
  if (!/^\d+$/.test(whole) || !/^\d*$/.test(fraction)) throw new Error(`Invalid amount: "${value}"`);
  return BigInt(whole + fraction.slice(0, decimals).padEnd(decimals, "0"));
}
```
{% endcode %}

{% hint style="info" %}
`MIN_SHARES` filters events on your side, after delivery, so every filtered-out event has still been billed. To cut CU usage, narrow the subscription itself. For example, add `filters.to` with your own wallet address to receive only incoming Stock Tokens.
{% endhint %}
{% endstep %}

{% step %}
### Wire it into an entry point

`index.js` loads the configuration, starts the watcher, and prints each alert on one line with a Blockscout link:

{% code title="index.js" overflow="wrap" %}
```js
import "dotenv/config";
import { STOCK_TOKENS } from "./tokens.js";
import { watchStockTokens } from "./stocks.js";

const ICONS = { MINT: "🟢", BURN: "🔴", TRANSFER: "🔁", REVERTED: "⚠️ " };
const short = (addr) => `${addr.slice(0, 6)}…${addr.slice(-4)}`;
const minShares = process.env.MIN_SHARES || "0";

console.log(`📈 Watching ${STOCK_TOKENS.map((t) => t.symbol).join(", ")} on Robinhood Chain`);
console.log(`   Minimum size: ${minShares} shares (Ctrl+C to stop)\n`);

const stream = watchStockTokens(STOCK_TOKENS, {
  apiKey: process.env.GETBLOCK_API_KEY,
  minShares,
  onStatus: (line) => console.log(`[stream] ${line}`),
  onAlert(alert) {
    const route =
      alert.type === "MINT" ? `→ ${short(alert.to)}`
      : alert.type === "BURN" ? `← ${short(alert.from)}`
      : `${short(alert.from)} → ${short(alert.to)}`;
    console.log(
      `${ICONS[alert.type]} ${alert.type.padEnd(8)} ${alert.shares.padStart(14)} ${alert.symbol.padEnd(5)} ${route}  block ${alert.block}  ${alert.time}`
    );
    console.log(`   ${alert.explorer}`);
  },
});

process.on("SIGINT", () => {
  stream.close();
  process.exit(0);
});
```
{% endcode %}
{% endstep %}

{% step %}
### Run it

Start the feed:

```bash
npm start
```

Expected output:

{% code title="terminal" overflow="wrap" %}
```bash
📈 Watching TSLA, NVDA, AAPL, MSFT, AMZN, GOOGL, META, SPY on Robinhood Chain
   Minimum size: 0 shares (Ctrl+C to stop)

[stream] Connected. Subscribing...
[stream] Subscribed to TSLA (0xfe0c936f10efc334656d12eca813485a)
[stream] Subscribed to NVDA (0x45b88931680341c6f1ee54f86d51797f)
[stream] Subscribed to AAPL (0xc94d10e73e600d8e49e49ca134cafa51)
[stream] Subscribed to MSFT (0xb43f97729a0ab8c7bf6f8d7fbf00413a)
[stream] Subscribed to AMZN (0x762167727b60ad868a9db83489f889e8)
[stream] Subscribed to GOOGL (0x47dc503af50cc59eb0b98659ebf7469f)
[stream] Subscribed to META (0x4101e05a19e5e9e71cbb0160f9d92bbe)
[stream] Subscribed to SPY (0x0d656978516d23e8881ab6053669a684)
   https://robinhoodchain.blockscout.com/tx/0x642f30488eec7df2075dd6182e5fc4e8b2b7ad981f04bcff3f71b5a1992b99c2
🔁 TRANSFER 0.015744307602595288 SPY   0x3e58…71d3 → 0x8366…0951  block 116498881  2026-11-17T10:12:49.000Z
   https://robinhoodchain.blockscout.com/tx/0x642f30488eec7df2075dd6182e5fc4e8b2b7ad981f04bcff3f71b5a1992b99c2
🔁 TRANSFER 0.000029616030939966 SPY   0x8366…0951 → 0x3e58…71d3  block 116498881  2026-11-17T10:12:49.000Z
   https://robinhoodchain.blockscout.com/tx/0x642f30488eec7df2075dd6182e5fc4e8b2b7ad981f04bcff3f71b5a1992b99c2
```
{% endcode %}

{% hint style="info" %}
Set `MIN_SHARES=10` in `.env` to see only larger movements. When you stop the app with`Ctrl+C`, the socket closes, and every subscription on it ends with it.
{% endhint %}
{% endstep %}
{% endstepper %}

## Troubleshooting

<table data-search="false"><thead><tr><th>Symptom</th><th>Likely cause</th><th>Fix</th></tr></thead><tbody><tr><td><code>Disconnected (1006)</code> straight after start, repeating</td><td>The handshake was rejected: missing or invalid API key, or 50 connections already open</td><td>Check that <code>GETBLOCK_API_KEY</code> is your <code>gb_…</code> key, not an RPC access token. Close other stream connections.</td></tr><tr><td><code>Subscribe failed for …: -32001 subscription limit exceeded</code></td><td>Your account already has 15 active subscriptions, across all networks</td><td>Shorten the watchlist, or close other stream clients that use the same key.</td></tr><tr><td><code>Subscribe failed for …: -32602 invalid tokenContract</code></td><td>A watchlist address is not a valid 20-byte hex address</td><td>Re-copy the contract address from Blockscout.</td></tr><tr><td>CU usage is higher than expected</td><td>Every delivered event is billed, including ones <code>MIN_SHARES</code> filters out locally, and each resubscribe costs 10 CU</td><td>Narrow server-side filters (<code>from</code>, <code>to</code>) and avoid needless reconnect loops.</td></tr></tbody></table>

## Conclusion

You built a live Stock Token feed for Robinhood Chain with GetBlock EVM Data Stream. The app opens one subscription per token to the `erc20Transfers` topic, classifies mints and burns by the zero address, and converts 18-decimal amounts with BigInt. It also deduplicates by `eventId` and `revision` and reconnects with backoff, so it keeps the feed accurate without polling RPC.

## Resources

* [EVM Data Stream overview](../evm-data-stream/overview.md)
* [Getting Started with EVM Data Stream](../evm-data-stream/getting-started.md)
* [erc20Transfers topic reference](../evm-data-stream/api-reference/erc20transfers-evm-data-stream.md)
* [Handling Chain Reorganizations](../evm-data-stream/handling-chain-reorganizations.md)
* [Get a GetBlock API key](https://account.getblock.io/)
* [EVM Data Stream playground](https://account.getblock.io/stream/playground)
* [Robinhood Chain explorer (Blockscout)](https://robinhoodchain.blockscout.com/)
* [Project repo](https://github.com/GetBlock-io/guides/tree/main/robinhood-stock-token-stream)
