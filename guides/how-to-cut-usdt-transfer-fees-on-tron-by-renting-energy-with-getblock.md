---
description: >-
  This guide shows you how to cut USDT (TRC-20) transfer fees on TRON by renting
  Energy with the GetBlock TRON Energy API instead of burning TRX
---

# How to Cut USDT Transfer Fees on TRON by Renting Energy with GetBlock

Every USDT transfer on TRON executes a smart contract, and smart contracts consume Energy. A wallet with no Energy pays by burning TRX: about 6.5 TRX when the recipient already holds USDT, and about 13 TRX when it never has. That is often more than the transfer itself e.g sending 1 USDT to an empty wallet can cost 4 USDT in fees. Staking TRX to earn your own Energy locks up thousands of TRX and takes 14 days to unwind. For exchanges, payment services, and wallets that move USDT all day, burning TRX on every transfer is the single largest operating cost on TRON.


The **GetBlock TRON Energy API** rents Energy to any TRON address for a USD fee, delivered on-chain in seconds. A 1-hour rental that covers one USDT transfer costs about 1.2–1.4 TRX instead of the 6.5 TRX you would burn.

In this guide, you'll build a **Node.js tool that plans a USDT transfer, compares the burn cost against a live rental quote, rents only the Energy that's missing, and then sends the USDT** with no TRX burned for Energy.

| Burning TRX                                    | Renting Energy with GetBlock                        |
| ---------------------------------------------- | --------------------------------------------------- |
| \~6.5 TRX per USDT transfer (\~13 TRX to new holders) | \~1.2–1.4 TRX for a 1-hour rental             |
| Wallet must hold TRX for fees                  | Pay from your GetBlock Credits balance              |
| Staking locks TRX for 14 days                  | No TRX locked — Energy is delegated to you          |
| Fee is only known after the transaction        | Free `price-estimate` before you commit             |

### What you'll build

A `planUsdtTransfer(from, to, amount)` function and CLI that:

1. Estimates how much Energy the USDT transfer will consume, by simulating it on-chain.
2. Reads how much Energy the sender already has and calculates the shortfall.
3. Prices the shortfall two ways: burning TRX at the live network rate, or renting it with `price-estimate`.
4. Returns **RENT**, **BURN**, or **NO RENTAL NEEDED** — and with `--rent`, activates the sender if needed, rents the Energy, and waits for the order.
5. With `--send`, sends the USDT and prints the fee receipt.

{% hint style="warning" %}
**Rent for the shortest duration that covers your transfers.** The price is per unit of Energy for the whole rental period, so it grows fast. For 65,000 Energy, a `1h` rental costs about 1.2 TRX, but a `1d` rental costs about 8.6 TRX — more than the 6.5 TRX you'd burn. This guide always rents for `1h`.
{% endhint %}

{% hint style="info" %}
Rented Energy is added on top of what the address already has, and it recovers over 24 hours like staked Energy. The tool only rents the shortfall, plus a 1,000-unit buffer because a delegation can land 1 unit short (65,000 ordered shows as 64,999 on-chain).
{% endhint %}

## Prerequisites

* **Node.js 18+**
* A [**GetBlock account**](https://account.getblock.io) with Credits on your balance (click **Credits** in the dashboard header, then **Top up**)
* A **GetBlock API key** — generate one under **Settings → API Keys** (Tron Energy accepts any GetBlock API key)
* A **GetBlock TRON Mainnet endpoint** — **RPC → Shared Node**, create an endpoint with Protocol **TRON**, Network **Mainnet**, API Interface **Full Node REST**
* A **TRON wallet** holding some USDT (and its private key) — only needed to actually send
* Basic JavaScript knowledge

## Project Setup

{% stepper %}
{% step %}
#### Create the project and install dependencies

```bash
mkdir tron-energy-fee-saver && cd tron-energy-fee-saver
npm init -y
npm pkg set type=module
npm install tronweb dotenv
```

[TronWeb](https://tronweb.network/) talks to your GetBlock TRON node; `dotenv` loads your keys.
{% endstep %}

{% step %}
#### Configure your keys

Create a `.env` file in the project root:

{% code title=".env" overflow="wrap" %}
```bash
# GetBlock API key — Settings → API Keys in the GetBlock dashboard
GETBLOCK_ENERGY_KEY=your_getblock_api_key

# GetBlock TRON Mainnet endpoint (Full Node REST)
TRON_RPC_URL=https://shared.eu-central-1.getblock.io/your_access_token

# Only needed for --send: private key of the <from> address
SENDER_PRIVATE_KEY=
```
{% endcode %}

{% hint style="danger" %}
Your API key spends your GetBlock Credits, and the private key controls your funds. Add `.env` to `.gitignore` and never commit or share either value.
{% endhint %}
{% endstep %}

{% step %}
#### Build the TRON Energy API client

This module wraps the five endpoints. Every write request carries an `Idempotency-Key`, so retrying after a timeout replays the original response instead of charging you twice.

{% code title="energy.js" overflow="wrap" %}
```javascript
// energy.js — thin client for the GetBlock TRON Energy API
import "dotenv/config";
import { randomUUID } from "node:crypto";

const BASE_URL = "https://services.getblock.io/v1/tron-energy";

/**
 * Call a TRON Energy endpoint and return the unwrapped `data` object.
 * Write endpoints get an Idempotency-Key so a retry never double-charges.
 */
async function energyRequest(method, path, body, idempotencyKey) {
  const apiKey = process.env.GETBLOCK_ENERGY_KEY;
  if (!apiKey) throw new Error("Set GETBLOCK_ENERGY_KEY in your .env file");

  const headers = {
    Authorization: `Bearer ${apiKey}`,
    "Content-Type": "application/json",
  };
  if (idempotencyKey) headers["Idempotency-Key"] = idempotencyKey;

  const res = await fetch(`${BASE_URL}/${path}`, {
    method,
    headers,
    body: body ? JSON.stringify(body) : undefined,
  });

  const json = await res.json().catch(() => ({}));
  if (!res.ok || !json.data) {
    throw new Error(`TRON Energy ${path} failed (HTTP ${res.status}): ${json.error ?? "empty response"}`);
  }
  return { status: res.status, ...json.data }; // unwrap the { data, error } envelope
}

/** Free quote: what would `volume` Energy (or Bandwidth) cost for `duration`? */
export function priceEstimate(volume, duration = "1h", resourceType = "energy") {
  return energyRequest("POST", "price-estimate", { resourceType, volume, duration });
}

/** Rent Energy to `address`. Charges your balance. */
export function delegateEnergy(address, volume, duration = "1h", idempotencyKey = randomUUID()) {
  return energyRequest(
    "POST",
    "delegate-energy",
    { target_address: address, volume, duration },
    idempotencyKey
  );
}

/** Create a never-used address on-chain so it can receive delegations. Charges your balance. */
export function activateAddress(address, idempotencyKey = randomUUID()) {
  return energyRequest("POST", "address-activate", { target_address: address }, idempotencyKey);
}

/** Look up an order placed with delegate-energy or delegate-bandwidth. */
export function getOrder(orderId) {
  return energyRequest("GET", `orders/${orderId}`);
}

/**
 * Poll an order until it is delivered (`charged`). Stops on any other status.
 * Orders return `success` (200) when delivered at once, or `accepted` (202) while pending.
 */
export async function waitForOrder(orderId, { timeoutMs = 60_000, intervalMs = 3_000 } = {}) {
  const deadline = Date.now() + timeoutMs;
  while (Date.now() < deadline) {
    const order = await getOrder(orderId);
    if (order.status === "charged") return order;
    if (!["accepted", "success"].includes(order.status)) {
      throw new Error(`Order ${orderId} has unexpected status "${order.status}"`);
    }
    await new Promise((r) => setTimeout(r, intervalMs));
  }
  throw new Error(`Order ${orderId} not confirmed after ${timeoutMs / 1000}s`);
}
```
{% endcode %}
{% endstep %}

{% step %}
#### Read on-chain state through your GetBlock TRON node

This module answers three questions — how much Energy the transfer needs, how much the sender has, and what burning costs — and sends the USDT.

The Energy estimate simulates the transfer with `triggerconstantcontract`. If the sender doesn't hold enough USDT to simulate, it falls back to the known costs: \~65K to an address that already holds USDT, \~131K to one that never has (the recipient's balance slot has to be created).

{% code title="tron.js" overflow="wrap" %}
```javascript
// tron.js — on-chain reads and the USDT transfer, through a GetBlock TRON node
import "dotenv/config";
import { TronWeb } from "tronweb";

export const USDT = "TR7NHqjeKQxGTCi8q8ZY4pL8otSzgjLj6t"; // USDT (TRC-20) on TRON mainnet
export const SUN_PER_TRX = 1_000_000;

const rpcUrl = process.env.TRON_RPC_URL;
if (!rpcUrl) throw new Error("Set TRON_RPC_URL in your .env file");

export const tronWeb = new TronWeb({
  fullHost: rpcUrl.replace(/\/$/, ""),
  privateKey: process.env.SENDER_PRIVATE_KEY || undefined,
});

/** Current burn prices set by TRON governance, in SUN per unit. */
export async function getBurnPrices() {
  const params = await tronWeb.trx.getChainParameters();
  const get = (key) => params.find((p) => p.key === key)?.value ?? 0;
  return { energySun: get("getEnergyFee"), bandwidthSun: get("getTransactionFee") };
}

/** An address exists on-chain once it has been activated (received TRX or been created). */
export async function isActivated(address) {
  const account = await tronWeb.trx.getAccount(address);
  return Boolean(account.address);
}

/** Energy and Bandwidth the address can spend right now. */
export async function getResources(address) {
  const r = await tronWeb.trx.getAccountResources(address);
  return {
    energy: (r.EnergyLimit ?? 0) - (r.EnergyUsed ?? 0),
    bandwidth:
      (r.freeNetLimit ?? 0) - (r.freeNetUsed ?? 0) + (r.NetLimit ?? 0) - (r.NetUsed ?? 0),
  };
}

/** USDT balance in whole tokens (6 decimals). */
export async function usdtBalance(address) {
  const { constant_result } = await tronWeb.transactionBuilder.triggerConstantContract(
    USDT,
    "balanceOf(address)",
    {},
    [{ type: "address", value: address }],
    address
  );
  return Number(BigInt("0x" + constant_result[0])) / 1e6;
}

/**
 * Energy a USDT transfer will consume. Simulates the call when the sender holds
 * enough USDT; otherwise falls back to the known costs (~65K to an address that
 * already holds USDT, ~131K to one that never has).
 */
export async function estimateUsdtEnergy(from, to, amount) {
  if ((await usdtBalance(from)) >= amount) {
    const sim = await tronWeb.transactionBuilder.triggerConstantContract(
      USDT,
      "transfer(address,uint256)",
      {},
      [
        { type: "address", value: to },
        { type: "uint256", value: Math.round(amount * 1e6) },
      ],
      from
    );
    if (sim.result?.result && sim.energy_used) return { energy: sim.energy_used, source: "simulated" };
  }
  const recipientHasUsdt = (await usdtBalance(to)) > 0;
  return { energy: recipientHasUsdt ? 65_000 : 131_000, source: "typical" };
}

/** Send USDT from SENDER_PRIVATE_KEY's address and return the fee receipt. */
export async function sendUsdt(to, amount) {
  const contract = await tronWeb.contract().at(USDT);
  const txid = await contract
    .transfer(to, Math.round(amount * 1e6))
    .send({ feeLimit: 30 * SUN_PER_TRX }); // cap the burn at 30 TRX if something is off

  // The receipt is available once the transaction is in a block (~3s). Read it from
  // the full node (not walletsolidity) so a single Full Node REST endpoint is enough.
  for (let i = 0; i < 20; i++) {
    const info = await tronWeb.fullNode.request("wallet/gettransactioninfobyid", { value: txid }, "post");
    if (info.id) {
      return {
        txid,
        result: info.receipt?.result ?? "SUCCESS",
        energyUsed: info.receipt?.energy_usage_total ?? 0,
        bandwidthUsed: info.receipt?.net_usage ?? 0,
        trxBurned: ((info.receipt?.energy_fee ?? 0) + (info.receipt?.net_fee ?? 0)) / SUN_PER_TRX,
      };
    }
    await new Promise((r) => setTimeout(r, 3_000));
  }
  return { txid, result: "PENDING" };
}
```
{% endcode %}
{% endstep %}

{% step %}
#### Decide whether to rent or burn

The planner combines both modules. It only calls `price-estimate`, so planning is always free.

Bandwidth is deliberately **not** rented: a USDT transfer needs \~345 bytes, every account gets 600 free Bandwidth per day, and burning the rest costs \~0.35 TRX — less than the 1,000-unit minimum rental (\~1 TRX).

{% code title="planner.js" overflow="wrap" %}
```javascript
// planner.js — decide whether renting Energy beats burning TRX for one USDT transfer
import { priceEstimate } from "./energy.js";
import {
  getBurnPrices,
  getResources,
  isActivated,
  estimateUsdtEnergy,
  SUN_PER_TRX,
} from "./tron.js";

const MIN_ENERGY_ORDER = 32_000; // API minimum for delegate-energy
const ENERGY_BUFFER = 1_000; // delegations can land 1 unit short; don't burn for the remainder
const USDT_TX_BANDWIDTH = 345; // bytes for a typical TRC-20 transfer

/**
 * Work out what a USDT transfer from `from` to `to` will cost with and without
 * rented Energy. Only `price-estimate` is called, so planning is free.
 */
export async function planUsdtTransfer(from, to, amount, duration = "1h") {
  // Sequential on purpose: keeps us under per-second limits on shared nodes.
  const activated = await isActivated(from);
  const burn = await getBurnPrices();
  const estimate = await estimateUsdtEnergy(from, to, amount);
  const available = activated ? await getResources(from) : { energy: 0, bandwidth: 0 };

  const shortfall = Math.max(0, estimate.energy - available.energy);
  // Bandwidth is not rented: the 1,000-unit minimum costs more than burning ~345 bytes.
  const bandwidthBurnTrx =
    available.bandwidth >= USDT_TX_BANDWIDTH ? 0 : (USDT_TX_BANDWIDTH * burn.bandwidthSun) / SUN_PER_TRX;
  const burnTrx = (shortfall * burn.energySun) / SUN_PER_TRX;

  const plan = {
    from,
    to,
    amount,
    activated,
    energyNeeded: estimate.energy,
    energySource: estimate.source,
    energyAvailable: available.energy,
    bandwidthAvailable: available.bandwidth,
    shortfall,
    burnTrx,
    bandwidthBurnTrx,
    rentVolume: 0,
    rentTrx: 0,
    rentUsd: 0,
    decision: "no-rent",
  };
  if (shortfall === 0) return plan; // already has enough Energy

  const rentVolume = Math.max(MIN_ENERGY_ORDER, Math.ceil((shortfall + ENERGY_BUFFER) / 1_000) * 1_000);
  const quote = await priceEstimate(rentVolume, duration);
  plan.rentVolume = rentVolume;
  plan.rentTrx = Number(quote.trx);
  plan.rentUsd = Number(quote.price_usd);
  plan.decision = plan.rentTrx < burnTrx ? "rent" : "burn";
  return plan;
}
```
{% endcode %}
{% endstep %}

{% step %}
#### Wire it into a CLI

The entry point is a dry run by default. `--rent` spends your GetBlock balance; `--send` moves real USDT.

{% code title="index.js" overflow="wrap" %}
```javascript
// index.js — plan, rent, and send a cheap USDT transfer
//   node index.js <from> <to> <amount>                 dry run (free)
//   node index.js <from> <to> <amount> --rent          rent Energy if cheaper than burning
//   node index.js <from> <to> <amount> --rent --send   rent, then send the USDT
import { planUsdtTransfer } from "./planner.js";
import { activateAddress, delegateEnergy, waitForOrder } from "./energy.js";
import { getResources, sendUsdt, tronWeb } from "./tron.js";

const [from, to, amountArg] = process.argv.slice(2).filter((a) => !a.startsWith("--"));
const rent = process.argv.includes("--rent");
const send = process.argv.includes("--send");

if (!from || !to || !amountArg) {
  console.error("Usage: node index.js <from> <to> <amount> [--rent] [--send]");
  process.exit(1);
}
const amount = Number(amountArg);
const fmt = (n) => n.toLocaleString("en-US");

const plan = await planUsdtTransfer(from, to, amount);

console.log(`\nUSDT transfer: ${amount} USDT  ${from} → ${to}`);
console.log(`  Sender activated      ${plan.activated ? "yes" : "no (activation ≈ 2.5 TRX)"}`);
console.log(`  Energy needed         ${fmt(plan.energyNeeded)} (${plan.energySource})`);
console.log(`  Energy available      ${fmt(plan.energyAvailable)}`);
console.log(`  Bandwidth available   ${fmt(plan.bandwidthAvailable)}`);
console.log(`  Burn cost (no rental) ${plan.burnTrx.toFixed(2)} TRX`);
if (plan.rentVolume) {
  const saved = (1 - plan.rentTrx / plan.burnTrx) * 100;
  console.log(`  Rental cost (1h)      ${plan.rentTrx.toFixed(2)} TRX ($${plan.rentUsd.toFixed(2)}) for ${fmt(plan.rentVolume)} Energy`);
  console.log(`  Decision              ${plan.decision.toUpperCase()}${plan.decision === "rent" ? ` — saves ${saved.toFixed(0)}%` : ""}`);
} else {
  console.log("  Decision              NO RENTAL NEEDED — sender already has the Energy");
}

if (rent && plan.decision === "rent") {
  if (!plan.activated) {
    const activation = await activateAddress(from);
    console.log(`\nActivated ${from} for ${activation.price_trx} TRX ($${activation.price_usd})`);
  }

  const order = await delegateEnergy(from, plan.rentVolume, "1h");
  console.log(`\nRented ${fmt(order.volume)} Energy — order ${order.orderId}, status ${order.status}`);
  const confirmed = await waitForOrder(order.orderId);
  console.log(`  Order ${confirmed.status}: $${confirmed.price_usd}, txid ${confirmed.txid}`);

  const after = await getResources(from);
  console.log(`  Energy now available  ${fmt(after.energy)}`);
}

if (send) {
  if (!process.env.SENDER_PRIVATE_KEY) throw new Error("Set SENDER_PRIVATE_KEY to send");
  if (tronWeb.defaultAddress.base58 !== from) throw new Error("SENDER_PRIVATE_KEY does not match <from>");

  const receipt = await sendUsdt(to, amount);
  console.log(`\nSent ${amount} USDT — txid ${receipt.txid}`);
  console.log(`  Result                ${receipt.result}`);
  console.log(`  Energy used           ${fmt(receipt.energyUsed ?? 0)}`);
  console.log(`  Bandwidth used        ${fmt(receipt.bandwidthUsed ?? 0)}`);
  console.log(`  TRX burned            ${(receipt.trxBurned ?? 0).toFixed(6)} TRX`);
}
```
{% endcode %}
{% endstep %}

{% step %}
#### Run it

Start with a free dry run:

```bash
node index.js <FROM_ADDRESS> <TO_ADDRESS> 1
```

Expected output when the sender holds USDT and the recipient never has (the simulation returns the real \~130K cost):

```bash
USDT transfer: 1 USDT  TNXoiAJ3dct8Fjg4M9fkLFh9S2v9TXc32G → TXBWFBixKeoDSXZUbMC3WwityH7SAxFmUY
  Sender activated      yes
  Energy needed         130,285 (simulated)
  Energy available      65,043
  Bandwidth available   600
  Burn cost (no rental) 6.52 TRX
  Rental cost (1h)      1.41 TRX ($0.46) for 67,000 Energy
  Decision              RENT — saves 78%
```

The sender already had 65,043 Energy, so the tool rents only the missing 67,000 instead of the full 131,000.

When you're happy with the plan, rent and send:

```bash
node index.js <FROM_ADDRESS> <TO_ADDRESS> 1 --rent --send
```

The `--rent` step prints the order as it moves to `charged`. For example, renting 65,000 Energy to an address that had none:

```bash
Rented 65,000 Energy — order 0a6d59c4-9486-45b1-bf3f-74d075ad9b4c, status success
  Order charged: $0.40, txid 63285f35942a0a8552fa7664b5fd8d4c2140640e9c766fe44fda7e01c97233df
  Energy now available  64,999
```

Note the 1-unit rounding — that's why the planner adds a 1,000-unit buffer.

The `--send` step then prints the transfer receipt. `TRX burned` should be `0.000000` for Energy; if the sender has already used its 600 free Bandwidth today, up to \~0.35 TRX is burned for Bandwidth.

{% hint style="info" %}
Rented Energy stays on the address for the full hour. If you send several USDT transfers inside that window, rent enough for all of them in one order. The unit price is the same, and you make one API call and one on-chain delegation instead of several.
{% endhint %}
{% endstep %}
{% endstepper %}

## Understanding the response

`delegate-energy` returns the order as soon as it's on-chain:

```json
{
  "data": {
    "orderId": "0a6d59c4-9486-45b1-bf3f-74d075ad9b4c",
    "status": "success",
    "target_address": "TXBWFBixKeoDSXZUbMC3WwityH7SAxFmUY",
    "resource_type": "energy",
    "volume": 65000,
    "duration": "1h",
    "price_usd": "0.40",
    "trx_spent": 2,
    "txid": "63285f35942a0a8552fa7664b5fd8d4c2140640e9c766fe44fda7e01c97233df"
  }
}
```

| Field           | Type    | What it tells you                                                                  |
| --------------- | ------- | ---------------------------------------------------------------------------------- |
| orderId         | string  | ID to pass to `GET /orders/{order_id}`                                             |
| status          | string  | `success` (200, delivered) or `accepted` (202, pending); `charged` once billed     |
| target\_address | string  | Address that received the Energy                                                   |
| resource\_type  | string  | `energy` or `bandwidth`                                                            |
| volume          | integer | Energy delegated (on-chain it can show 1 unit less)                                |
| duration        | string  | Rental period — Energy returns to GetBlock when it ends                            |
| price\_usd      | string  | What was charged — can differ from the `price-estimate` quote                      |
| trx\_spent      | number  | TRX spent on the delegation; `0` while `accepted`                                  |
| txid            | string  | The delegation transaction — look it up on [Tronscan](https://tronscan.org)        |

## Troubleshooting

| Symptom                                                         | Likely cause                                               | Fix                                                                                               |
| --------------------------------------------------------------- | ---------------------------------------------------------- | ------------------------------------------------------------------------------------------------- |
| `HTTP 401` `Missing or invalid Authorization header`            | Missing, mistyped, or revoked Energy API key               | Check `GETBLOCK_ENERGY_KEY`; create a new key under **Settings → API Keys**                      |
| `HTTP 400` `energy volume must be between 32000 and 5000000`    | Rental below the 32,000 minimum                            | The planner rounds up to 32,000 — check you didn't lower `MIN_ENERGY_ORDER`                       |
| `HTTP 400` `invalid duration, allowed: 15m, 1h, 1d, 3d, 7d, 14d` | Unsupported duration such as `3h` or `12h`                 | Use one of the listed values                                                                      |
| `HTTP 400` `Invalid target_address`                             | Address isn't valid base58 TRON (`T…`, 34 characters)      | Copy the address again; hex (`41…`) addresses aren't accepted                                     |
| `HTTP 429` from the TRON node                                   | Too many requests per second on a public or shared node    | Use your GetBlock endpoint in `TRON_RPC_URL`; the planner already sends calls one at a time       |
| Decision is `BURN`                                              | Rental quote is higher than the burn cost right now        | Prices follow network demand — retry later, or send without renting                               |
| Transfer still burns TRX for Energy                             | Recipient never held USDT, so it used \~131K, not \~65K    | The simulation handles this when the sender holds USDT; otherwise rent 131,000                    |
| `SENDER_PRIVATE_KEY does not match <from>`                      | The key in `.env` belongs to a different address           | Use the private key of the `<from>` address                                                       |

## Conclusion

You built a tool that prices a USDT transfer before sending it, rents only the Energy the sender is missing through the GetBlock TRON Energy API, and sends the transfer without burning TRX for Energy. It combines on-chain simulation through a GetBlock TRON node, live rental quotes from `price-estimate`, and idempotent `delegate-energy` orders confirmed with `orders/{order_id}`.

### Resources

* [TRON Energy overview](https://docs.getblock.io/tron-energy/introduction)
* [TRON Fee Model](https://docs.getblock.io/tron-energy/tron-fee-model)
* [TRON Energy API reference](https://docs.getblock.io/tron-energy/api-reference)
* [GetBlock TRON API reference](https://docs.getblock.io/api-reference/tron-trx)
* [Get an API key](https://account.getblock.io)
* [Project Repo](https://github.com/GetBlock-io/guides/tree/main/tron-energy-fee-saver)
