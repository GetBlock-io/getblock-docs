---
description: >-
  Learn how to optimize your Solana transactions with Jito bundles and priority
  fees
hidden: true
---

# How To Optimize Solana Transactions With Jito Bundles

Though Solana is the fastest blockchain at the moment, it is still subject to delays. For example, in Dec, 2024, the day [PENGU (memecoin)](https://www.binance.com/en-NG/square/post/17720434201329) launched, thousands of Superteam members received an airdrop, and there was heavy network congestion. Hence, the transaction was not easy, and the gas fee went up.&#x20;

During this congestion, block production can't always keep up. The network's capacity (measured in Transactions Per Second) can become saturated. This time, transactions with the tips or higher gas prices are usually considered.&#x20;

To address this issue, Jito Labs, a builder of high-performance MEV infrastructure for Solana, operates a Solana validator client that enables a unique feature called [Bundles](https://docs.jito.wtf/lowlatencytxnsend/#what-are-bundles).

_In this guide, you'll learn:_

* _What Jito bundles are,_&#x20;
* _The comparison between normal transactions and using Jito bundles,_&#x20;
* _A walkthrough on how to implement them in your dApp_
* _Best practices._

### What Are Jito Bundles?

Jito bundles are groups of transactions (up to 5) that are executed **sequentially** and **atomically**. They're submitted directly to Jito's Block Engine, which [auctions](https://docs.jito.wtf/lowlatencytxnsend/#what-is-the-auction) block space to the highest-tipping bundles and forwards winning bundles to Jito-Solana validators.

Key properties:

* **Atomic Execution**: All transactions in the bundle execute together, or none do. No partial failures.
* **Guaranteed Ordering**: Transactions execute in the exact sequence you specify.
* **Private Submission**: Bundles aren't visible in the public mempool until they're included in a block. This prevents MEV attacks.
* **Tip-Based Priority**: Instead of priority fees, you pay a "tip" directly to the validator who includes your bundle.

{% hint style="info" %}
The tip is set as an instruction and add to the last transaction
{% endhint %}

#### How Jito Bundles Work

1. You create signs and bundle all the transactions via dApp
2. Your dApp sends this bundle transaction to the Jito Block engine
3. Jito block engine forwards the bundle to the Jito-Solana validators
4. The Jito-solana  uses BundleStage to position the bundles before executing
5. The bundles is excuted when the **Jito-Solana validators are producing blocks**

#### The Tip Mechanism

Jito tips are **direct SOL transfers** to the validator. This creates a direct incentive for validators to include your bundle.

**How tips work:**

1. You include a tip transaction as the **last transaction** in your bundle
2. The tip is a simple SOL transfer to one of Jito's official tip accounts
3. If your bundle lands, the tip goes to the validator who included it
4. If your bundle doesn't land, **you pay nothing**

**Official Jito Tip Accounts:**

{% code overflow="wrap" %}
```javascript
const JITO_TIP_ACCOUNTS = [
"DttWaMuVvTiduZRnguLF7jNxTgiMBZ1hyAumKUiL2KRL",
"Cw8CFyM9FkoMi7K7Crf6HNQqf4uEMzpKw6QNghXLvLkY",
"3AVi9Tg9Uo68tJfuvoKvqKNWKkC5wPdSSdeBnizKZ6jT",
"ADuUkR4vqLUMWXxW9gh6D6L8pMSawimctcNZ5pGwDcEt",
"ADaUMid9yfUytqMBgopwjb2DTLSokTSzL1zt6iGPaS49",
"96gYZGLnJYVFmbjzopPSU6QiEV5fGqZNyN9nmNhvrZU5",
"DfXygSm4jCyNCybVYYK6DwvWqjKee8pbDmJGcLWNDXjh",
"HFqU5x63VTqvQss8hp11i4wVV8bD44PvwucfZ2bU7gRe"
];
```
{% endcode %}

#### Tip Amount Guidelines

| Level       | SOL Amount   | Use Case                                   |
| ----------- | ------------ | ------------------------------------------ |
| Minimum     | 0.0001       | Testing, low-priority operations           |
| Standard    | 0.001        | Normal DeFi operations                     |
| Competitive | 0.005 - 0.01 | Time-sensitive swaps, moderate competition |
| High        | 0.01 - 0.05  | High-value trades, liquidations            |
| Aggressive  | 0.05+        | Arbitrage, high-competition scenarios      |

### Comparison: Normal Transactions vs Jito Bundles

| Feature                 | Normal Transaction | Jito Bundle                  |
| ----------------------- | ------------------ | ---------------------------- |
| **Mempool Visibility**  | Public             | Private until execution      |
| **MEV Protection**      | ❌ None             | ✅ Protected                  |
| **Atomic Execution**    | ❌ Single TX only   | ✅ Up to 5 TXs                |
| **Guaranteed Ordering** | ❌ No               | ✅ Yes                        |
| **Cost on Failure**     | Pay base fee       | ✅ No cost                    |
| **Network**             | Devnet + Mainnet   | ⚠️ Mainnet only              |
| **Best For**            | Simple transfers   | DeFi, arbitrage, complex ops |
| **Complexity**          | Low                | Medium                       |

#### When to Use Jito Bundle

* DeFi trading (swaps, arbitrage, liquidations)
* Multi-step operations that must execute together
* Large trades vulnerable to sandwich attacks
* Any operation where ordering and privacy matter
* Failed transactions would be costly







