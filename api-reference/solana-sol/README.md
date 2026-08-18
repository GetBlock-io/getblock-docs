---
description: >-
  GetBlock Solana RPC API documentation: JSON-RPC reference, WebSocket
  subscriptions, and connection guides for mainnet, devnet & alpenglow endpoints
---

# Solana(SOL)

Solana is a high-throughput Layer 1 blockchain launched in 2020 by Solana Labs, built around a single global state machine rather than sharding or rollups. It pairs Proof of History, a verifiable delay function that orders transactions before consensus, with Proof of Stake for validator selection, and executes non-conflicting transactions concurrently through the Sealevel runtime. The design targets sub-second confirmation for payments, order-book trading, and consumer applications that cannot tolerate multi-second block times.

### Key Features

* **Proof of History**: A cryptographic clock orders transactions before consensus is reached
* **400ms Slot Time**: Slots are scheduled roughly every 400 milliseconds, four per leader
* **Parallel Execution**: The Sealevel runtime executes transactions concurrently when account access sets do not overlap
* **Local Fee Markets**: Prioritization fees are priced per account, so congestion on one program does not raise fees network-wide
* **SPL Token Standard**: Fungible and non-fungible tokens are handled by the on-chain Token and Token-2022 programs
* **Token Extensions**: Token-2022 adds transfer fees, confidential transfers, and metadata without custom programs
* **Versioned Transactions**: Address lookup tables allow transactions to reference more accounts than the legacy format permits
* **Stake-Weighted QoS**: Transaction forwarding capacity is allocated in proportion to validator stake

{% hint style="info" %}
_TECHNICAL DISCLAIMER: AUTHORITATIVE JSON-RPC API SPECIFICATION._

_GetBlock's RPC API reference documentation is provided exclusively for informational purposes and to optimize the developer experience. The canonical and normative specification for Solana JSON-RPC methods is solely maintained and published through the official Solana documentation at_ [_solana.com/docs_](https://solana.com/docs)_. This resource constitutes the sole authoritative reference implementation of the JSON-RPC 2.0 protocol interface across Solana validator clients._
{% endhint %}

### Network Information

| Property        | Value                                         |
| --------------- | --------------------------------------------- |
| Network Name    | Solana Mainnet Beta                           |
| Genesis Hash    | 5eykt4UsFv8P8NJdTREpY1vzqKqZKvdpKuc147dw2N9d  |
| Native Currency | SOL (1 SOL = 1,000,000,000 lamports)          |
| Slot Time       | \~400 milliseconds                            |
| Slots per Epoch | 432,000                                       |
| Consensus       | Proof of History + Tower BFT (Proof of Stake) |
| Runtime         | Sealevel (parallel execution)                 |
| Address Format  | Base58-encoded Ed25519 public keys            |
| Token Programs  | SPL Token and Token-2022                      |

### Base URL

{% tabs %}
{% tab title="Frankfurt, Germany" %}
```bash
https://shared.eu-central-1.getblock.io
```
{% endtab %}

{% tab title="New York, USA" %}
```bash
https://shared.us-east-1.getblock.io
```
{% endtab %}

{% tab title="Singapore, Singapore" %}
```bash
https://shared.ap-southeast-1.getblock.io/
```
{% endtab %}
{% endtabs %}

### Supported Networks

| Network   | JSON RPC | WSS | MEV-protected(JSON-RPC) | Frankfurt, Germany | New York, USA | Singapore, Singapore |
| --------- | -------- | --- | ----------------------- | ------------------ | ------------- | -------------------- |
| Mainnet   | ✅        | ✅   | ✅                       | ✅                  | ✅             | ✅                    |
| Devnet    | ✅        | ✅   | ❌                       | ✅                  | ❌             | ❌                    |
| Alpenglow | ✅        | ✅   | ❌                       | ✅                  | ❌             | ❌                    |

### Quickstart

In this section, you will learn how to make your first call with either:

* Axios
* Python

Before you begin, you must have already installed [`npm`](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm) or [`yarn`](https://classic.yarnpkg.com/lang/en/docs/install) on your local machine (for the Axios example) or Python and pip (for the Python example).

{% tabs %}
{% tab title="Javascript(Axios)" %}
{% stepper %}
{% step %}
### Setup project

Create and initialize a new project:

```bash
mkdir solana-api-quickstart
cd solana-api-quickstart
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

Add the following code to `index.js`:

{% code title="index.js" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "getSlot",
    "params": [],
    "id": "getblock.io"
});

const config = {
    method: 'post',
    url: 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/',
    headers: {
        'Content-Type': 'application/json'
    },
    data: data
};

axios(config)
    .then(response => console.log(JSON.stringify(response.data)))
    .catch(error => console.log(error));
```
{% endcode %}

Replace `<ACCESS-TOKEN>` with your actual access token from GetBlock.
{% endstep %}

{% step %}
### Run the script

```bash
node index.js
```

Expected output (example):

```json
{
    "jsonrpc": "2.0",
    "result": 434717885,
    "id": "getblock.io"
}
```
{% endstep %}
{% endstepper %}
{% endtab %}

{% tab title="Python(Request)" %}
{% stepper %}
{% step %}
### Set up the project folder

```bash
mkdir solana-api-quickstart
cd solana-api-quickstart
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
### Create the script

Create a file called `main.py` with the following code (replace `<ACCESS-TOKEN>` with your GetBlock access token):

```python
import requests
import json

url = "https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "getSlot",
    "params": [],
    "id": "getblock.io"
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
```
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

### Available API Methods

GetBlock provides access to standard Solana JSON-RPC methods for the Solana network.

#### Account Methods

| Method                            | Description                                        |
| --------------------------------- | -------------------------------------------------- |
| getAccountInfo                    | Returns all information associated with an account |
| getBalance                        | Returns the lamport balance of an account          |
| getMultipleAccounts               | Returns account information for a list of Pubkeys  |
| getProgramAccounts                | Returns all accounts owned by a program            |
| getMinimumBalanceForRentExemption | Returns the minimum balance for rent exemption     |

#### Token Methods

| Method                     | Description                                         |
| -------------------------- | --------------------------------------------------- |
| getTokenAccountBalance     | Returns the token balance of an SPL Token account   |
| getTokenAccountsByOwner    | Returns all SPL Token accounts held by an owner     |
| getTokenAccountsByDelegate | Returns all SPL Token accounts by approved delegate |
| getTokenLargestAccounts    | Returns the 20 largest accounts for a token mint    |
| getTokenSupply             | Returns the total supply of an SPL Token mint       |

#### Transaction Methods

| Method                      | Description                                             |
| --------------------------- | ------------------------------------------------------- |
| sendTransaction             | Submits a signed transaction to the cluster             |
| simulateTransaction         | Simulates a transaction without broadcasting it         |
| getTransaction              | Returns transaction details for a confirmed signature   |
| getSignatureStatuses        | Returns the status of a list of signatures              |
| getSignaturesForAddress     | Returns signatures referencing an address               |
| getTransactionCount         | Returns the transaction count since genesis             |
| getFeeForMessage            | Returns the fee for a compiled transaction message      |
| getLatestBlockhash          | Returns the latest blockhash and its expiry height      |
| isBlockhashValid            | Reports whether a blockhash is still valid              |
| getRecentPrioritizationFees | Returns recent prioritization fee samples               |
| requestAirdrop              | Requests lamports from the faucet on Devnet and Testnet |

#### Block Methods

| Method                      | Description                                       |
| --------------------------- | ------------------------------------------------- |
| getBlock                    | Returns identity and transaction data for a block |
| getBlocks                   | Returns confirmed block slots in a range          |
| getBlocksWithLimit          | Returns a limited list of confirmed block slots   |
| getBlockHeight              | Returns the current block height                  |
| getBlockTime                | Returns the estimated production time of a block  |
| getBlockCommitment          | Returns stake-weighted commitment for a block     |
| getBlockProduction          | Returns leader slot and block production counts   |
| getFirstAvailableBlock      | Returns the lowest non-purged confirmed block     |
| getRecentPerformanceSamples | Returns recent throughput samples                 |
| minimumLedgerSlot           | Returns the lowest slot in the node's ledger      |

#### Cluster Methods

| Method                 | Description                                   |
| ---------------------- | --------------------------------------------- |
| getSlot                | Returns the current slot                      |
| getSlotLeader          | Returns the current slot leader               |
| getSlotLeaders         | Returns slot leaders for a slot range         |
| getLeaderSchedule      | Returns the leader schedule for an epoch      |
| getEpochInfo           | Returns information about the current epoch   |
| getEpochSchedule       | Returns the cluster epoch schedule parameters |
| getClusterNodes        | Returns nodes discovered through gossip       |
| getVoteAccounts        | Returns current and delinquent vote accounts  |
| getHealth              | Returns the health status of the node         |
| getVersion             | Returns the node software version             |
| getIdentity            | Returns the identity Pubkey of the node       |
| getGenesisHash         | Returns the genesis hash of the cluster       |
| getHighestSnapshotSlot | Returns the highest snapshot slots            |
| getMaxRetransmitSlot   | Returns the highest retransmit-stage slot     |
| getMaxShredInsertSlot  | Returns the highest shred-insert slot         |

#### Economics Methods

| Method                    | Description                                       |
| ------------------------- | ------------------------------------------------- |
| getSupply                 | Returns total and circulating SOL supply          |
| getInflationRate          | Returns inflation rates for the current epoch     |
| getInflationGovernor      | Returns the cluster inflation governor            |
| getInflationReward        | Returns inflation rewards for a list of addresses |
| getStakeMinimumDelegation | Returns the minimum stake delegation              |

### Support

For technical support and questions:

* Support: [support@getblock.io](mailto:support@getblock.io)

### See Also

* [Solana Developer Documentation](https://solana.com/docs)
* [Solana JSON-RPC Specification](https://solana.com/docs/rpc/http)
* [Solana Block Explorer](https://explorer.solana.com/)
* [SPL Token Program Documentation](https://spl.solana.com/token)
* [Anchor Framework Documentation](https://www.anchor-lang.com/)
