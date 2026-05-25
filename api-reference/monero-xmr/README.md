# Monero(XMR)

Monero (XMR) is the leading privacy-focused cryptocurrency by market capitalization and a foundational implementation of the CryptoNote protocol. Originally proposed in 2013 by Nicolas van Saberhagen and launched in 2014, Monero uses ring signatures, stealth addresses, and RingCT (Ring Confidential Transactions) to make transactions untraceable on public block explorers — a property that distinguishes it from transparent ledgers like Bitcoin and Ethereum. The network is secured by the ASIC-resistant RandomX Proof-of-Work algorithm, which keeps mining accessible to commodity CPU hardware and counters centralization pressure. Monero is the canonical choice for use cases that require fungibility and on-chain confidentiality.

### Key Features

* **Ring Signatures**: Sender identity is obfuscated by mixing the real signer with decoy outputs drawn from prior transactions on the chain
* **Stealth Addresses**: Each transaction generates a unique one-time recipient address, hiding which wallet ultimately receives the funds
* **RingCT (Ring Confidential Transactions)**: Transaction amounts are cryptographically hidden — only sender and recipient know how much XMR moved
* **RandomX Proof-of-Work**: CPU-friendly, ASIC-resistant mining algorithm that preserves decentralization
* **Atomic Units**: 1 XMR = 10¹² atomic units (12 decimals) — the smallest unit in monerod's accounting
* **Bulletproofs**: Zero-knowledge range proofs that drastically reduce transaction size while preserving privacy
* **Dynamic Block Size**: Blocks scale up under load and shrink under quiet periods, smoothing fee volatility
* **Tail Emission**: Fixed 0.6 XMR per block emission after the main supply schedule, ensuring perpetual miner incentives
* **Adaptive Hard Forks**: Network-wide upgrades every \~6 months keep the protocol responsive to cryptographic research
* **Tor and I2P Support**: First-class integration with anonymity networks at the node and wallet layer

{% hint style="info" %}
**TECHNICAL DISCLAIMER: AUTHORITATIVE MONEROD RPC SPECIFICATION**

GetBlock’s Monero API reference documentation is provided exclusively for informational purposes and to optimize the developer experience. The canonical and normative specification for the monerod daemon RPC interface is maintained by the Monero Project and published at [docs.getmonero.org](https://docs.getmonero.org/rpc-library/monerod-rpc/) with the upstream source at [github.com/monero-project/monero-docs](https://github.com/monero-project/monero-docs). For protocol-level details, transaction structure, and cryptographic primitives, consult the official Monero documentation at [getmonero.org](https://www.getmonero.org/).
{% endhint %}

{% hint style="warning" %}
**DAEMON RPC ONLY — NO WALLET RPC**

GetBlock exposes the monerod **daemon RPC** for reading blockchain data, broadcasting transactions, and consensus-level queries. GetBlock does **not** expose `monero-wallet-rpc`, which handles private-key operations such as creating addresses, signing transfers, and querying wallet balances. Wallet RPC must be run locally against your own wallet file — never expose it on a managed RPC endpoint.

This is standard practice across all reputable Monero RPC providers and is required for user safety.
{% endhint %}

## Network Information

| Property           | Value                                                                                  |
| ------------------ | -------------------------------------------------------------------------------------- |
| Network Name       | Monero Mainnet                                                                         |
| Native Currency    | Monero (XMR)                                                                           |
| Decimals           | 12 (1 XMR = 10¹² atomic units)                                                         |
| Block Time         | \~2 minutes (120 seconds target)                                                       |
| Consensus          | Proof-of-Work (RandomX algorithm)                                                      |
| Privacy Primitives | Ring signatures, stealth addresses, RingCT, Bulletproofs                               |
| Address Format     | Base58 — standard (`4…` / `8…`), subaddress (`8…`), integrated (`4…` with payment ID)  |
| Block Explorer     | [xmrchain.net](https://xmrchain.net/), [exploremonero.com](https://exploremonero.com/) |

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

All Monero JSON-RPC methods are called by sending a `POST` request to the base URL with a standard JSON-RPC 2.0 body. Non-JSON-RPC HTTP methods (such as `/get_height` and `/send_raw_transaction`) are accessed at their own URL paths under the same base URL.

## Supported Networks

| Network  | JSON-RPC | HTTP Endpoints | Frankfurt, Germany | New York, USA | Singapore, Singapore |
| -------- | -------- | -------------- | ------------------ | ------------- | -------------------- |
| Mainnet  | ✅        | ✅              | ✅                  | ✅             | ✅                    |
| Stagenet | ✅        | ✅              | ✅                  | ✅             | ✅                    |

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

Create and initialize a new project:

```bash
mkdir monero-api-quickstart
cd monero-api-quickstart
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
    "method": "get_block_count",
    "params": {},
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
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "count": 3194582,
        "status": "OK",
        "untrusted": false
    }
}
```

The `count` field is the current height of the Monero blockchain.
{% endstep %}
{% endstepper %}
{% endtab %}

{% tab title="Python (Requests)" %}
{% stepper %}
{% step %}
### Setup the project directory

```bash
mkdir monero-api-quickstart
cd monero-api-quickstart
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

Create a file called `main.py` with the following content:

{% code title="main.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "get_block_count",
    "params": {},
    "id": "getblock.io"
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
```
{% endcode %}

Replace `<ACCESS-TOKEN>` with your actual access token from GetBlock.
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

Monero exposes two parallel surfaces: standard **JSON-RPC methods** (the majority) and **non-JSON-RPC HTTP endpoints** for binary and high-performance operations. GetBlock provides access to both.

Methods marked **{disallowed}** are administrative operations that mutate node state. GetBlock blocks these on shared and public endpoints for safety. They are documented for completeness and may be available on dedicated nodes by request.

### JSON-RPC: Blocks & Chain

| Method                         | Description                                                        |
| ------------------------------ | ------------------------------------------------------------------ |
| get\_block\_count              | Returns the current height of the longest chain                    |
| on\_get\_block\_hash           | Returns the block hash at a given height                           |
| get\_block\_template           | Returns a block template for mining                                |
| submit\_block                  | Submits a mined block to the network                               |
| get\_last\_block\_header       | Returns the header of the most recent block                        |
| get\_block\_header\_by\_hash   | Returns a block header by its hash                                 |
| get\_block\_header\_by\_height | Returns a block header by height                                   |
| get\_block\_headers\_range     | Returns headers for a range of blocks                              |
| get\_block                     | Returns full block information by height or hash                   |
| get\_alternate\_chains         | Returns information about known alternative chains                 |
| get\_fee\_estimate             | Returns the current fee estimate for transactions                  |
| get\_coinbase\_tx\_sum         | Returns the cumulative coinbase transaction sum over a block range |

### JSON-RPC: Node Info & Network

| Method                 | Description                                                   |
| ---------------------- | ------------------------------------------------------------- |
| get\_info              | Returns general information about the node and network state  |
| get\_version           | Returns the version of the connected monerod instance         |
| hard\_fork\_info       | Returns information about the current and upcoming hard forks |
| sync\_info             | Returns information about node synchronization status         |
| get\_connections       | Returns the list of P2P connections of the node               |
| get\_output\_histogram | Returns a histogram of RingCT outputs by amount               |

### JSON-RPC: Transaction Pool

| Method               | Description                                                  |
| -------------------- | ------------------------------------------------------------ |
| get\_txpool\_backlog | Returns the current transaction pool backlog                 |
| relay\_tx            | Relays one or more transactions already in the pool to peers |
| flush\_txpool        | Removes transactions from the pool **{disallowed}**          |

### JSON-RPC: Bans & Administration

| Method    | Description                                     |
| --------- | ----------------------------------------------- |
| get\_bans | Returns the list of banned IPs **{disallowed}** |
| set\_bans | Bans one or more IPs **{disallowed}**           |
| banned    | Checks whether a given IP is banned             |

### Non-JSON-RPC HTTP Endpoints

These endpoints do **not** use the JSON-RPC envelope — they take a plain JSON body posted to their dedicated path.

| Endpoint                  | Description                                               |
| ------------------------- | --------------------------------------------------------- |
| /get\_height              | Returns the current node height (lightweight)             |
| /get\_transactions        | Returns one or more transactions by hash                  |
| /get\_alt\_blocks\_hashes | Returns hashes of known alternative-chain blocks          |
| /is\_key\_image\_spent    | Checks whether one or more key images have been spent     |
| /send\_raw\_transaction   | Broadcasts a serialized signed transaction to the network |
| /get\_outs                | Returns transaction outputs by index                      |
| /get\_limit               | Returns current peer-to-peer bandwidth limits             |
| /get\_net\_stats          | Returns network statistics for the node                   |
| /get\_peer\_list          | Returns the node's known peer list                        |
| /get\_public\_nodes       | Returns the list of advertised public peer nodes          |

## Support

For technical support and questions:

* **Support Email**: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [Official Monero Documentation](https://www.getmonero.org/)
* [Monero Docs — monerod RPC Reference](https://docs.getmonero.org/rpc-library/monerod-rpc/)
* [Monero Project on GitHub](https://github.com/monero-project/monero)
* [Monero Block Explorer (xmrchain)](https://xmrchain.net/)
* [monero-python (Python SDK)](https://github.com/monero-ecosystem/monero-python)
* [monero-javascript (JavaScript SDK)](https://github.com/monero-ecosystem/monero-javascript)
* [Monero StackExchange](https://monero.stackexchange.com/)
