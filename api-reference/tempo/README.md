---
description: >-
  GetBlock provides fast and reliable access to Tempo nodes via JSON-RPC API.
  Connect to the ARC network without running your own infrastructure.
---

# Tempo

Tempo is a Layer 1 blockchain purpose-built for stablecoin payments at scale, incubated by Stripe and Paradigm and launched on mainnet on March 18, 2026. It's designed from the ground up to handle high-throughput, low-cost real-world payment flows. Tempo introduces an unusual but coherent design choice: **no native gas token**.&#x20;

Instead of a volatile chain-native asset, transaction fees are paid in TIP-20 stablecoins (USD-denominated tokens), with a built-in stablecoin DEX maintaining liquidity. The chain runs on Paradigm's high-performance Reth execution client with deterministic sub-second finality (\~0.6-second blocks) and ships with dedicated payment lanes that reserve blockspace at the protocol level, so fees stay low even during congestion.&#x20;

The network is fully EVM-compatible, which means you can deploy any Solidity contract and extends the standard Ethereum JSON-RPC with three Tempo-specific namespaces (`tempo_*`, `consensus_*`, `admin_*`) for fork scheduling, consensus introspection, and validator administration.

### Key Features

* **No Native Gas Token**: Transaction fees are paid in TIP-20 stablecoins (USD-denominated) — not in a volatile native asset. The single most important integration consideration; see warning below
* **Deterministic Sub-Second Finality**: \~0.6 second blocks with no re-orgs — finalized blocks are guaranteed to be included in the chain
* **Dedicated Payment Lanes**: Protocol-level reservation of blockspace for payments keeps fees stable (\~$0.001 per transaction) even during network congestion
* **Built-in Stablecoin DEX**: Native decentralized exchange optimized for stablecoins and tokenized deposits, with maintained liquidity for fee conversion
* **EVM Compatibility**: Deploy Solidity / Vyper contracts with Hardhat, Foundry, ethers.js, viem, or web3.py — built on Paradigm's Reth client
* **Tempo Transactions (Type `0x54`)**: A Tempo-specific transaction type complementing standard EVM transaction types (`0x0`, `0x1`, `0x2`, `0x7702`)
* **Machine Payments Protocol**: Open standard co-authored with Stripe for AI-agent-to-service payments — extended to Visa cards and Bitcoin Lightning
* **Modern Wallet Signing**: Programmable smart accounts with gas sponsorship, batch transactions, scheduled payments, and passkey auth
* **Payments Metadata**: Structured memo fields for invoices and identifiers — reconcile against ERP systems without custom code
* **Opt-in Privacy**: Confidential balances and transfers designed with regulated issuers in mind, with auditability for compliance
* **Institutional Design Partners**: Visa, Mastercard, Deutsche Bank, Standard Chartered, Nubank, Revolut, Shopify, OpenAI, Anthropic, DoorDash, Klarna, and others helped shape the protocol

{% hint style="info" %}
**TECHNICAL DISCLAIMER: AUTHORITATIVE JSON-RPC SPECIFICATION**

_GetBlock's Tempo API reference documentation is provided exclusively for informational purposes and to optimize the developer experience. The standard Ethereum JSON-RPC base is specified by the Ethereum community at_ [_ethereum.org/developers/docs/apis/json-rpc_](https://ethereum.org/en/developers/docs/apis/json-rpc/)_. The canonical specification for Tempo's `tempo_*`, `consensus_*`, and `admin_*` namespaces, EVM differences, and protocol semantics is maintained by the Tempo team at_ [_docs.tempo.xyz_](https://docs.tempo.xyz/)_. For TIP-20 token standards, fee model details, and the Machine Payments Protocol, consult the_ [_Tempo protocol specifications_](https://docs.tempo.xyz/protocol)_._
{% endhint %}

{% hint style="warning" %}
**NO NATIVE GAS TOKEN — IMPORTANT**

Tempo has **no native gas token**. Several standard Ethereum JSON-RPC methods behave differently as a result:

* **`eth_getBalance`** always returns a large constant placeholder value (`0x9612084f...`), **NOT** an actual balance. Wallets and dApps that display this value as "ETH balance" will show meaningless huge numbers. Use **TIP-20 `balanceOf`** on a specific token contract to query real balances.
* **`eth_estimateGas`** estimates the gas allowance against the effective fee payer's **TIP-20 fee token balance**, not against a native token.
* **`eth_sendRawTransaction`** accepts standard EVM transaction types AND Tempo Transactions (type `0x54`).
* **Transaction `value` fields** in standard EVM transactions are effectively ignored — there is no native asset to transfer. Token transfers must go through TIP-20 contracts.

This is fundamentally different from chains where the native gas token is a stablecoin (such as Circle's Arc, where USDC is native with 6 decimals). On Tempo, there is **no chain-level native asset at all** — all value is held as TIP-20 tokens.
{% endhint %}

## Network Information

| Property          | Value                                                            |
| ----------------- | ---------------------------------------------------------------- |
| Network Name      | Tempo Mainnet                                                    |
| Mainnet Chain ID  | 4217 (`0x1079`)                                                  |
| Testnet           | Moderato (chain ID 42431, `0xa59f`)                              |
| Currency Unit     | USD (denomination only — no native gas token, see warning above) |
| Block Time        | \~0.6 seconds                                                    |
| Finality          | Deterministic instant finality (no re-orgs)                      |
| Smart Contract VM | EVM (Reth-based execution client)                                |
| EVM Compatible    | Yes (with documented behavior differences for balance/gas)       |
| Address Format    | Ethereum-style (`0x…`, 20 bytes)                                 |
| Block Explorer    | [explore.tempo.xyz](https://explore.tempo.xyz/)                  |
| Faucet (Testnet)  | `tempo_fundAddress` RPC method (Moderato testnet only)           |
| Mainnet Launch    | March 18, 2026                                                   |

## Base URL

{% tabs %}
{% tab title="Frankfurt, Germany" %}
```bash
https://go.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}

{% tab title="New York, USA" %}
```bash
https://go.getblock.us/<ACCESS-TOKEN>/
```
{% endtab %}

{% tab title="Singapore, Singapore" %}
```bash
https://go.getblock.asia/<ACCESS-TOKEN>/
```
{% endtab %}
{% endtabs %}

All Tempo JSON-RPC methods are called by sending a `POST` request to the base URL with a JSON-RPC 2.0 body. For real-time subscriptions (new blocks, logs, consensus events) use the WebSocket scheme: `wss://go.getblock.io/<ACCESS-TOKEN>/`.

## Supported Networks

| Network          | JSON-RPC | WSS | Frankfurt, Germany | New York, USA | Singapore, Singapore |
| ---------------- | -------- | --- | ------------------ | ------------- | -------------------- |
| Mainnet          | ✅        | ✅   | ✅                  | ✅             | ✅                    |
| Moderato Testnet | ✅        | ✅   | ✅                  | ✅             | ✅                    |

## Quickstart

In this section, you will learn how to make your first call with either:

* **Axios** (JavaScript / Node.js)
* **Python** (Requests library)

Before you begin, you must have already installed [`npm`](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm) or [`yarn`](https://classic.yarnpkg.com/lang/en/docs/install) on your local machine (for the Axios example) or Python and pip (for the Python example).

{% tabs %}
{% tab title="JavaScript (Axios)" %}
{% stepper %}
{% step %}
### Setup project

```bash
mkdir tempo-api-quickstart
cd tempo-api-quickstart
npm init --yes
```
{% endstep %}

{% step %}
### Install Axios

```bash
npm install axios
```
{% endstep %}

{% step %}
### Create file

Create a new file named `index.js`. This is where you will make your first call.
{% endstep %}

{% step %}
### Set ES module type

Set the ES module `"type": "module"` in your `package.json`.
{% endstep %}

{% step %}
### Add code

{% code title="index.js" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "eth_chainId",
    "params": [],
    "id": "getblock.io"
});

const config = {
    method: 'post',
    url: 'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers: {
        'Content-Type': 'application/json'
    },
    data: data
};

axios(config)
    .then(response => console.log(JSON.stringify(response.data, null, 2)))
    .catch(error => console.log(error));
```
{% endcode %}

Replace `<ACCESS-TOKEN>` with your actual GetBlock access token.
{% endstep %}

{% step %}
### Run the script

```bash
node index.js
```

Expected output:

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x1079"
}
```

`0x1079` (4217 in decimal) confirms you are connected to Tempo mainnet.
{% endstep %}
{% endstepper %}
{% endtab %}

{% tab title="Python (Requests)" %}
{% stepper %}
{% step %}
### Setup the project directory

```bash
mkdir tempo-api-quickstart
cd tempo-api-quickstart
```
{% endstep %}

{% step %}
### Create and activate a virtual environment

```bash
python -m venv venv
source venv/bin/activate

# On Windows, use venv\Scripts\activate
```
{% endstep %}

{% step %}
### Install requests

```bash
pip install requests
```
{% endstep %}

{% step %}
### Create script

{% code title="main.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "eth_chainId",
    "params": [],
    "id": "getblock.io"
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
```
{% endcode %}

Replace `<ACCESS-TOKEN>` with your actual GetBlock access token.
{% endstep %}

{% step %}
### Run the script

```bash
python main.py
```
{% endstep %}
{% endstepper %}
{% endtab %}
{% endtabs %}

## Available API Methods

Tempo exposes the full standard Ethereum JSON-RPC method set (with documented behavior differences on a few methods), plus three Tempo-specific namespaces.

### Ethereum: Blocks & Chain

| Method                | Description                                            |
| --------------------- | ------------------------------------------------------ |
| eth\_blockNumber      | Returns the current latest block number                |
| eth\_chainId          | Returns the chain ID (`0x1079` = 4217 for mainnet)     |
| eth\_getBlockByNumber | Returns block information by block number              |
| eth\_getBlockByHash   | Returns block information by block hash                |
| eth\_getBlockReceipts | Returns all transaction receipts for a given block     |
| eth\_syncing          | Returns sync status or `false` if node is fully synced |

### Ethereum: Account & State

| Method                   | Description                                                                             |
| ------------------------ | --------------------------------------------------------------------------------------- |
| eth\_getBalance          | **Modified** — returns a constant placeholder; use TIP-20 `balanceOf` for real balances |
| eth\_getCode             | Returns the contract bytecode at a given address                                        |
| eth\_getStorageAt        | Returns the value at a specific storage slot                                            |
| eth\_getTransactionCount | Returns the transaction count (nonce) for an account                                    |

### Ethereum: Transactions

| Method                     | Description                                                               |
| -------------------------- | ------------------------------------------------------------------------- |
| eth\_sendRawTransaction    | **Modified** — accepts standard EVM types AND Tempo Transactions (`0x54`) |
| eth\_getTransactionByHash  | Returns a transaction by its hash                                         |
| eth\_getTransactionReceipt | Returns the receipt for a transaction by hash                             |
| eth\_call                  | Executes a read-only call against contract state                          |
| eth\_estimateGas           | **Modified** — estimates against TIP-20 fee token balance, not native ETH |

### Ethereum: Gas, Fees, & Logs

| Method                    | Description                                               |
| ------------------------- | --------------------------------------------------------- |
| eth\_gasPrice             | Returns the current gas price                             |
| eth\_maxPriorityFeePerGas | Returns the suggested max priority fee per gas (EIP-1559) |
| eth\_feeHistory           | Returns historical base fees and priority fees            |
| eth\_getLogs              | Returns logs matching a given filter                      |

### WebSocket Subscriptions

| Method           | Description                                                         |
| ---------------- | ------------------------------------------------------------------- |
| eth\_subscribe   | Subscribes to events (`newHeads`, `logs`, `newPendingTransactions`) |
| eth\_unsubscribe | Cancels an existing subscription                                    |

### Network & Client Info

| Method              | Description                                                        |
| ------------------- | ------------------------------------------------------------------ |
| net\_version        | Returns the network ID (`4217` for mainnet)                        |
| net\_listening      | Returns `true` if the client is actively listening for connections |
| net\_peerCount      | Returns the number of peers connected to the node                  |
| web3\_clientVersion | Returns the client software version                                |
| web3\_sha3          | Returns the Keccak-256 hash of the given data                      |

### Tempo: Fork Schedule & Faucet (`tempo_*`)

| Method              | Description                                               |
| ------------------- | --------------------------------------------------------- |
| tempo\_forkSchedule | Returns the Tempo fork schedule and currently active fork |
| tempo\_fundAddress  | Mints test stablecoins to an address (**testnet-only**)   |

### Tempo: Consensus (`consensus_*`)

| Method                                | Description                                                                 |
| ------------------------------------- | --------------------------------------------------------------------------- |
| consensus\_getFinalization            | Get a finalized block by height or latest (**validator nodes only**)        |
| consensus\_getLatest                  | Latest consensus state snapshot (**validator nodes only**)                  |
| consensus\_subscribe                  | WebSocket subscription to consensus events (**validator nodes only**)       |
| consensus\_unsubscribe                | Cancels a consensus subscription (**validator nodes only**)                 |
| consensus\_getIdentityTransitionProof | DKG identity transition proofs for light clients (**validator nodes only**) |

### Tempo: Administration (`admin_*`)

| Method              | Description                                                           |
| ------------------- | --------------------------------------------------------------------- |
| admin\_validatorKey | Returns the node's ed25519 validator key (**self-hosted nodes only**) |

## Support

For technical support and questions:

* **Support Email**: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [Official Tempo Documentation](https://docs.tempo.xyz/)
* [Tempo RPC Reference](https://docs.tempo.xyz/protocol/rpc)
* [Tempo EVM Differences](https://docs.tempo.xyz/quickstart/evm-compatibility)
* [Tempo Protocol Specifications](https://docs.tempo.xyz/protocol)
* [TIP Standards (Tempo Improvement Proposals)](https://tips.sh/)
* [Ethereum JSON-RPC Specification](https://ethereum.org/en/developers/docs/apis/json-rpc/)
* [Tempo Block Explorer](https://explore.tempo.xyz/)
* [Tempo GitHub](https://github.com/tempoxyz)
* [Tempo Wallet](https://wallet.tempo.xyz/)
* [Machine Payments Protocol](https://docs.tempo.xyz/guide/machine-payments)
* [Foundry (cast / forge / anvil)](https://getfoundry.sh/)
* [Ethers.js Documentation](https://docs.ethers.org/)
* [Viem Documentation](https://viem.sh/)
