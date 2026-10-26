---
description: >-
  This guide teaches you how to screen incoming crypto deposits for money
  laundering, sanctions, and fraud exposure before crediting them, using the
  GetBlock Crypto AML API.
icon: screencast
---

# How to Screen Crypto Deposits for AML Risk with GetBlock Crypto AML API

Once a crypto deposit lands in your wallet, you can't send it back. If those funds came from a sanctioned entity, a hack, or a darknet market, the problem is now yours. A downstream exchange may freeze your account, a regulator may ask for your screening records, and you may be left holding tainted funds.&#x20;

A blocklist doesn't catch this, because risky funds rarely come straight from a flagged address. They pass through clusters, mixers, and intermediate wallets first. To catch them, you need to check the sender's full on-chain history and counterparties before you credit the deposit, and keep a record of every check.

In this guide, you'll learn how to build a deposit screener with the **GetBlock Crypto AML API**. It checks a sender wallet or an incoming transaction, turns the risk report into an **ACCEPT / REVIEW / REJECT** decision, and saves an audit record for every check.

### What you'll build

A `screenDeposit({ address | tx, network, asset })` function that:

1. Sends the sender wallet to `POST /v1/aml/wallet-check`, or the deposit transaction to `POST /v1/aml/tx-check`.
2. Maps the `0–100` risk score to the documented risk bands: `low`, `medium`, `high`, `very-high`.
3. Collects the red flags: FATF flags, sanctions exposure, high-risk clusters and counterparties, and token restrictions.
4. Returns **ACCEPT**, **REVIEW**, or **REJECT**, along with the reasons.
5. **Fails closed.** If the check errors, the deposit is held for REVIEW instead of being credited.
6. Adds the request, decision, and `reportId` to an audit log (`aml-audit.jsonl`).

## Prerequisites

* **Node.js 18+**: the project uses the built-in `fetch`.
* A [**GetBlock account**](https://account.getblock.io/): sign up with an email. No KYB or annual contract needed.
* **Prepaid credit balance**: each successful check is charged to your balance.
* A [**GetBlock API key**](https://account.getblock.io/settings/api-keys): used as the bearer token for Crypto AML requests.
* Basic JavaScript knowledge.

{% hint style="info" %}
Crypto AML currently supports **Ethereum (`eth`)**, **TRON (`trx`)**, **Bitcoin (`btc`)**, **Litecoin (`ltc`)**, and **Bitcoin Cash (`bch`)**. See the [Overview](../crypto-aml/overview.md) for the latest list.
{% endhint %}

## Project Setup

{% stepper %}
{% step %}
#### Set up the project

```bash
mkdir aml-deposit-screener && cd aml-deposit-screener
npm init -y
npm pkg set type=module
npm install dotenv
```

`type=module` lets you use `import` and top-level `await`. `dotenv` is the only dependency, since `fetch` is built into Node 18+.
{% endstep %}

{% step %}
#### Get your API key

1. Log in to [account.getblock.io](https://account.getblock.io/).
2. Top up your balance. Web checks and API checks both draw from the same prepaid credits.
3. Go to [**Settings → API Keys**](https://account.getblock.io/settings/api-keys) and create a key.
4. Save it in a `.env` file:

```bash
GETBLOCK_AML_KEY="your_api_key_here"
```

{% hint style="warning" %}
The API key decides which account's balance gets charged. Never commit it. Add `.env` to `.gitignore`, and rotate the key from the dashboard if it leaks.
{% endhint %}
{% endstep %}

{% step %}
#### Write the API caller

Create `aml.js`. This is the only file that talks to GetBlock. It unwraps the `{ data, error }` envelope and throws on any non-OK response.

{% code title="aml.js" overflow="wrap" %}
```js
// aml.js
import "dotenv/config";

const BASE_URL = "https://services.getblock.io/v1/aml";

export const NETWORKS = ["eth", "trx", "btc", "ltc", "bch"];

/**
 * POST a body to a Crypto AML endpoint and return the unwrapped `data` report.
 * Throws on any non-OK response so the caller can fail closed.
 */
async function amlRequest(path, body) {
  const apiKey = process.env.GETBLOCK_AML_KEY;
  if (!apiKey) throw new Error("Set GETBLOCK_AML_KEY in your .env file");

  const res = await fetch(`${BASE_URL}/${path}`, {
    method: "POST",
    headers: {
      Authorization: `Bearer ${apiKey}`,
      "Content-Type": "application/json",
    },
    body: JSON.stringify(body),
  });

  const json = await res.json().catch(() => ({}));
  if (!res.ok || !json.data) {
    throw new Error(`Crypto AML request failed (HTTP ${res.status}): ${json.error ?? "empty report"}`);
  }
  return json.data; // unwrap the { data, error } envelope
}

/** Screen a wallet across every supported asset on the network. */
export function checkWallet(address, network) {
  return amlRequest("wallet-check", { address, network: network.toLowerCase() });
}

/**
 * Screen one asset transfer inside a transaction.
 * On ETH and TRX, always pass `asset` — omitting it screens only the native coin.
 */
export function checkTransaction(tx, network, asset) {
  const body = { tx, network: network.toLowerCase() };
  if (asset) body.asset = asset.toLowerCase();
  return amlRequest("tx-check", body);
}
```
{% endcode %}
{% endstep %}

{% step %}
#### Write the decision policy

Create `policy.js`. It maps the score to the [documented risk bands](../crypto-aml/how-risk-scoring-works.md#risk-bands-and-thresholds) and then looks at the supporting evidence. The score alone isn't enough to make a decision. A wallet can have a medium score and still have a direct FATF flag, or belong to a high-risk cluster.

{% code title="policy.js" overflow="wrap" %}
```js
// policy.js

/** Map a 0–100 score onto the documented Crypto AML risk bands. */
export function riskBand(score) {
  if (score >= 75) return "very-high";
  if (score >= 50) return "high";
  if (score >= 25) return "medium";
  return "low";
}

// Categories that should never be auto-accepted, wherever they appear.
const CRITICAL = new Set([
  "sanctionsExposure",
  "terrorismFinancing",
  "weaponsProliferationFinancing",
  "childAbuse",
  "humanExploitation",
  "stolenAssets",
  "hacking",
  "extortionActivity",
  "darknetActivity",
]);

/**
 * Turn a wallet or transaction report into ACCEPT / REVIEW / REJECT.
 * Tune the thresholds to your own compliance policy.
 */
export function decide(report) {
  const score = report.overallRiskScore ?? report.riskScore ?? 0;
  const band = riskBand(score);
  const reject = [];
  const review = [];

  if (band === "very-high") reject.push(`risk score ${score} (very-high band)`);
  if (band === "high") review.push(`risk score ${score} (high band)`);

  // Wallet report: direct FATF-style flags on counterparties.
  for (const flag of report.breakdown?.fatf?.flags ?? []) {
    const label = `${flag.flow}bound ${flag.category} x${flag.count}`;
    (CRITICAL.has(flag.category) ? reject : review).push(label);
  }

  // Wallet report: issuer restrictions on a token (e.g. frozen USDT).
  for (const r of report.attribution?.assetRestrictions ?? []) {
    review.push(`asset restriction on ${r.asset}`);
  }

  // Wallet report: the address sits inside a risky cluster (high band or above).
  const cluster = report.attribution?.cluster;
  if (cluster?.score >= 50) {
    review.push(`member of high-risk cluster ${cluster.names?.join(", ") ?? ""} (score ${cluster.score})`.trim());
  }

  // Transaction report: no counterparties means the asset transfer wasn't found, so nothing was screened.
  const cp = report.counterparties;
  if (cp && !Object.keys(cp.in ?? {}).length && !Object.keys(cp.out ?? {}).length) {
    review.push("no counterparties returned — check the tx hash and asset");
  }

  // Transaction report: any counterparty scoring in the very-high band.
  for (const flow of ["in", "out"]) {
    for (const [addr, s] of Object.entries(report.counterparties?.[flow] ?? {})) {
      if (s >= 75) review.push(`${flow} counterparty ${addr} scores ${s}`);
    }
  }

  // Both reports: critical categories in the exposure composition.
  const exposure = report.breakdown?.exposure ?? report;
  for (const e of [...(exposure.activityExposure ?? []), ...(exposure.complianceExposure ?? [])]) {
    if (CRITICAL.has(e.category) && e.share > 0) review.push(`${e.category} exposure ${e.share}%`);
  }

  const decision = reject.length ? "REJECT" : review.length ? "REVIEW" : "ACCEPT";
  return { decision, score, band, reasons: [...reject, ...review] };
}
```
{% endcode %}

{% hint style="info" %}
A category's `share` is its percentage of the **exposure composition**. It is **not** a risk score, and it doesn't tell you how much that category raised the headline score. That's why the policy only uses `share` to decide whether to send a deposit for review, and never adds shares together into a score.
{% endhint %}
{% endstep %}

{% step %}
#### Build the screener

Create `screener.js`. It runs the right check, applies the policy, and **fails closed**: if the check fails, the deposit is held for review instead of being credited. It also adds every result to an audit log. The transaction report doesn't include the `tx`, `network`, or `asset` you sent, so the log saves the request together with the decision.

{% code title="screener.js" overflow="wrap" %}
```js
// screener.js
import { appendFile } from "node:fs/promises";
import { checkWallet, checkTransaction, NETWORKS } from "./aml.js";
import { decide } from "./policy.js";

const AUDIT_LOG = "aml-audit.jsonl";

/**
 * Screen an incoming deposit before crediting it.
 * Pass `{ address, network }` for a wallet or `{ tx, network, asset }` for a transaction.
 * Fails CLOSED: if the check errors, the deposit is held for REVIEW.
 */
export async function screenDeposit(input) {
  const network = input.network?.toLowerCase();
  const kind = input.tx ? "transaction" : "wallet";
  let result;

  try {
    if (!NETWORKS.includes(network)) {
      throw new Error(`Unsupported network "${input.network}". Use one of: ${NETWORKS.join(", ")}`);
    }

    const report = input.tx
      ? await checkTransaction(input.tx, network, input.asset)
      : await checkWallet(input.address, network);

    result = { ...decide(report), reportId: report.reportId ?? null };
  } catch (err) {
    result = { decision: "REVIEW", score: null, band: "unknown", reasons: [`screening failed: ${err.message}`], reportId: null };
  }

  // The tx report does not echo the request, so store it with the decision.
  const record = { checkedAt: new Date().toISOString(), kind, request: input, ...result };
  await appendFile(AUDIT_LOG, JSON.stringify(record) + "\n");

  return record;
}
```
{% endcode %}

{% hint style="warning" %}
The audit log can contain customer wallet addresses. Add `aml-audit.jsonl` to `.gitignore`, and in production write these records to your compliance database instead.
{% endhint %}
{% endstep %}

{% step %}
#### Wire it into an entry point

Create `index.js`. It's a small CLI that screens either a wallet or a transaction, and exits with a non-zero code for anything other than a clean ACCEPT.

{% code title="index.js" overflow="wrap" %}
```js
// index.js
import { screenDeposit } from "./screener.js";

// Usage:
//   node index.js wallet <address> <network>
//   node index.js tx <txHash> <network> [asset]
const [mode, target, network, asset] = process.argv.slice(2);

if (!["wallet", "tx"].includes(mode) || !target || !network) {
  console.log("Usage: node index.js wallet <address> <network>");
  console.log("       node index.js tx <txHash> <network> [asset]");
  process.exit(1);
}

const input = mode === "tx" ? { tx: target, network, asset } : { address: target, network };
const r = await screenDeposit(input);

console.log(`Target:    ${target} (${network}${asset ? `/${asset}` : ""})`);
console.log(`Score:     ${r.score ?? "n/a"} (${r.band})`);
console.log(`Reasons:   ${r.reasons.length ? "\n  - " + r.reasons.join("\n  - ") : "none"}`);
console.log(`Report ID: ${r.reportId ?? "n/a"}`);
console.log(`Decision:  ${r.decision}`);

// Non-zero exit lets CI jobs or queues act on anything that isn't a clean ACCEPT.
process.exit(r.decision === "ACCEPT" ? 0 : 2);
```
{% endcode %}

In a real service, run the screener inside your deposit handler, before you update the user's balance:

```js
// inside your deposit webhook / confirmation handler
const { decision, reasons, reportId } = await screenDeposit({
  tx: deposit.txHash,
  network: "trx",
  asset: "usdt",
});

if (decision === "ACCEPT") await creditBalance(deposit);
else await holdForCompliance(deposit, { decision, reasons, reportId });
```
{% endstep %}

{% step %}
#### Run it

Screen a wallet:

```bash
node index.js wallet 0x8589427373D6D84E98730D7795D8f6f8731FDA16 eth
```

Expected output for the wallet above. It scores in the high band but has no direct flags, so it goes to manual review:

```bash
Target:    0x8589427373D6D84E98730D7795D8f6f8731FDA16 (eth)
Score:     70 (high)
Reasons:
  - risk score 70 (high band)
Report ID: 6aaaabeb-133d-4cec-98fd-7e61330658b9
Decision:  REVIEW
```

Now screen an address on the OFAC sanctions list:

```bash
node index.js wallet 0x0330070FD38Ec3bB94F58FA55D40368271E9e54A eth
```

Expected output:

```bash
Target:    0x0330070FD38Ec3bB94F58FA55D40368271E9e54A (eth)
Score:     100 (very-high)
Reasons:
  - risk score 100 (very-high band)
  - asset restriction on usdc
  - asset restriction on usdt
  - member of high-risk cluster OFAC SDN AMNOKGANG TECH 12-03-2026 (score 70)
Report ID: 17256fe8-fb42-46ad-8115-e93187ce3a44
Decision:  REJECT
```

Here, the exposure, external-report, FATF, and token-flag components all score `0`. The sanctions signal comes only from **attribution**: the address belongs to an OFAC SDN cluster, and USDT and USDC have restricted it. A policy that only looked at exposure or FATF flags would miss this, which is why `decide()` also checks the cluster and asset restrictions.

Finally, screen a USDT deposit that this sanctioned address sent:

```bash
node index.js tx 0x2a8df19623bfbd437d2da47b5e680ab94b60236d004e2bd5b6cf2e34be978ed9 eth usdt
```

Expected output:

```bash
Target:    0x2a8df19623bfbd437d2da47b5e680ab94b60236d004e2bd5b6cf2e34be978ed9 (eth/usdt)
Score:     100 (very-high)
Reasons:
  - risk score 100 (very-high band)
  - in counterparty 0x0330070fd38ec3bb94f58fa55d40368271e9e54a scores 100
  - sanctionsExposure exposure 100%
Report ID: n/a
Decision:  REJECT
```

The transaction report names the sender (`in`) and the receiver (`out`), each with its own score. The sanctioned sender scores `100`, and all of the transfer's exposure is `sanctionsExposure`. Transaction reports don't include a `reportId`, which is why the audit log saves the request alongside the decision.

Each run also adds one line to `aml-audit.jsonl`:

```json
{"checkedAt":"2026-10-26T11:52:41.000Z","kind":"wallet","request":{"address":"0x0330070FD38Ec3bB94F58FA55D40368271E9e54A","network":"eth"},"decision":"REJECT","score":100,"band":"very-high","reasons":["risk score 100 (very-high band)","asset restriction on usdc","asset restriction on usdt","member of high-risk cluster OFAC SDN AMNOKGANG TECH 12-03-2026 (score 70)"],"reportId":"17256fe8-fb42-46ad-8115-e93187ce3a44"}
```

{% hint style="info" %}
Risk scores are estimates, and they change over time. The same address can score differently later as new on-chain activity, attribution data, and sanctions feeds come in. Save the `reportId` and timestamp you based each decision on, so you can explain it later.
{% endhint %}
{% endstep %}
{% endstepper %}

## Understanding the response

Both endpoints return the report inside a `data` envelope. These are the fields the screener uses:

<table data-search="false"><thead><tr><th>Field</th><th>Type</th><th>What it tells you</th></tr></thead><tbody><tr><td><code>schemaVersion</code></td><td>string</td><td>Version of the response schema (currently <code>"1.0"</code>). Check it if your integration needs to handle future versions.</td></tr><tr><td><code>overallRiskScore</code></td><td>number <code>0–100</code></td><td>Wallet reports only. The headline score: <code>0–24</code> low, <code>25–49</code> medium, <code>50–74</code> high, <code>75–100</code> very high.</td></tr><tr><td><code>riskScore</code></td><td>number <code>0–100</code></td><td>Transaction reports only. The headline score for the asset transfer you screened.</td></tr><tr><td><code>breakdown.exposure</code></td><td>object</td><td>The wallet's exposure profile. Contains its own <code>score</code> and the <code>activityExposure</code>, <code>complianceExposure</code>, <code>counterpartyExposure</code>, and <code>unclassifiedExposure</code> arrays.</td></tr><tr><td><code>*Exposure[].share</code></td><td>number</td><td>Each category's percentage of the exposure composition. This is <strong>not</strong> a risk score.</td></tr><tr><td><code>complianceExposure[].authorities</code></td><td>array</td><td>For <code>sanctionsExposure</code>, the sanctions exposure split by authority (<code>UK</code>, <code>EU</code>, <code>UN</code>).</td></tr><tr><td><code>breakdown.externalReports</code></td><td>object</td><td>Risk backed by confirmed external reports, with its own <code>score</code>.</td></tr><tr><td><code>breakdown.fatf.flags</code></td><td>array</td><td>Direct flags on counterparties. Each one has a <code>flow</code> (<code>in</code>/<code>out</code>), a <code>category</code>, and a <code>count</code>.</td></tr><tr><td><code>breakdown.tokenFlags</code></td><td>object</td><td>Restrictions on specific tokens, such as an issuer freeze on USDT.</td></tr><tr><td><code>attribution.owners</code></td><td>string[]</td><td>Known entities believed to control the wallet. Only present when available.</td></tr><tr><td><code>attribution.cluster</code></td><td>object</td><td>Cluster <code>names</code>, <code>score</code>, and exposure. A wallet with little activity of its own can still be risky because of the cluster it belongs to.</td></tr><tr><td><code>attribution.assetRestrictions</code></td><td>array</td><td>Asset restrictions linked to the wallet.</td></tr><tr><td><code>counterparties.in</code> / <code>.out</code></td><td>object <code>address → score</code></td><td>Transaction reports only. The addresses that sent or received the screened transfer, with each one's risk score.</td></tr><tr><td><code>reportId</code></td><td>string</td><td>Wallet reports only. The unique ID of this screening. Save it with your decision for audits.</td></tr></tbody></table>

Optional arrays, such as `flags`, `owners`, `authorities`, and `unclassifiedExposure`, only appear when there is data for them. If one is missing, the check still worked. For every category value, see the [Taxonomy](../crypto-aml/taxonomy.md).

## Troubleshooting

<table data-search="false"><thead><tr><th>Symptom</th><th>Likely cause</th><th>Fix</th></tr></thead><tbody><tr><td><code>screening failed: Set GETBLOCK_AML_KEY</code></td><td><code>.env</code> is missing or in the wrong folder</td><td>Copy <code>.env.example</code> to <code>.env</code> in the project root and add your key.</td></tr><tr><td><code>HTTP 401</code> / <code>403</code></td><td>Invalid, revoked, or missing API key</td><td>Create a new key at <a href="https://account.getblock.io/settings/api-keys">Settings → API Keys</a>.</td></tr><tr><td><code>HTTP 402</code> <code>insufficient_balance</code></td><td>Not enough prepaid credits for another check</td><td>Top up your balance in the GetBlock dashboard. Failed checks aren't charged.</td></tr><tr><td><code>HTTP 400</code> <code>Supported networks: bch, btc, eth, ltc, trx</code></td><td>The network isn't supported (for example, <code>sol</code>, <code>bsc</code>)</td><td>Use one of the five supported network values.</td></tr><tr><td><code>HTTP 400</code> on <code>tx-check</code></td><td>The hash isn't 64 hex characters, or <code>asset</code> doesn't exist on that network</td><td>Check the hash and use a valid asset: <code>eth</code>/<code>usdt</code>/<code>usdc</code> on ETH, <code>trx</code>/<code>usdt</code> on TRX.</td></tr><tr><td>A USDT deposit scores low, but you expected it to be flagged</td><td><code>asset</code> was left out, so only the native coin was screened</td><td>Pass <code>asset: "usdt"</code> for token deposits.</td></tr><tr><td><code>riskScore: 0</code> with empty <code>counterparties</code> (REVIEW: "no counterparties returned")</td><td>The transaction has no transfer of the <code>asset</code> you requested, so nothing was screened</td><td>Check the hash and make sure <code>asset</code> matches the transfer you're crediting.</td></tr><tr><td>Score looks low but decision is <code>REVIEW</code></td><td>A cluster, counterparty, FATF flag, or token restriction triggered the policy</td><td>This is expected. Check <code>reasons</code> and look at the full report in the dashboard.</td></tr><tr><td>The same address scores differently on different days</td><td>New on-chain activity or updated attribution and sanctions data</td><td>This is expected. Base each decision on the <code>reportId</code> you saved, not on a new check.</td></tr></tbody></table>

## Conclusion

You built a deposit screener that calls the GetBlock Crypto AML `wallet-check` and `tx-check` endpoints, maps the score to the documented risk bands, and combines it with FATF flags, cluster and counterparty risk, and token restrictions to return ACCEPT, REVIEW, or REJECT. It fails closed when a check can't run and saves each decision with its `reportId`, so you can explain every deposit decision later.

### Resources

* [Crypto AML overview](https://docs.getblock.io/crypto-aml/overview)
* [How risk scoring works](https://docs.getblock.io/crypto-aml/how-risk-scoring-works)
* [Run check via API](https://docs.getblock.io/crypto-aml/run-check-via-api)
* [AML risk taxonomy](https://docs.getblock.io/crypto-aml/taxonomy)
* [Run a check in the dashboard](https://docs.getblock.io/crypto-aml/run-a-check-in-the-dashboard)
* [Get an API key](https://account.getblock.io/settings/api-keys)
* [AML Deposit Screener Repo](https://github.com/GetBlock-io/guides/tree/main/aml-deposit-screener)
