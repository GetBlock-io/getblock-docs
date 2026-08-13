---
description: >-
  GetBlock provides fast and reliable access to Kaia nodes via JSON-RPC API.
  Connect to the Kaia network without running your own infrastructure.
---

# Kaia (KAIA)

Kaia is an EVM-compatible Layer 1 blockchain formed by the merger of Klaytn and Finschia, backed by the Kakao and LINE ecosystems. It is a hard fork of the Klaytn chain, so the chain ID remains `8217`, while the native token is now **KAIA** (the rebrand of KLAY). Kaia targets roughly 1-second blocks with immediate finality, using a BFT consensus operated by a governance council of validators.

Kaia is EVM-compatible and exposes the standard Ethereum JSON-RPC method set through the `eth_` namespace, so Ethereum contracts and tooling — Hardhat, Foundry, ethers.js, viem, and web3.py — run without modification. Kaia also exposes a native `kaia_` namespace (the rebrand of Klaytn's `klay_`) with methods that have no Ethereum equivalent, such as account-key lookups, consensus and council information, and block reward distribution.

## Key Features

* **EVM Compatibility**: Standard Ethereum JSON-RPC via the `eth_` namespace; deploy Solidity contracts and use Hardhat, Foundry, ethers.js, viem, and web3.py without modification
* **Native `kaia_` Namespace**: Kaia-specific methods for account keys, consensus and council data, and rewards, alongside the `eth_` namespace
* **BFT Consensus with Immediate Finality**: A governance council of validators finalizes blocks in roughly one second, with no reorgs
* **Flexible Account Keys**: Accounts decouple keys from addresses, supporting public, weighted multisig, and role-based keys
* **Fee Delegation**: Kaia natively supports fee-delegated transactions, where a fee payer covers a user's gas
* **KAIA Native Token**: KAIA pays gas; the base unit is kei (1 KAIA = 10^18 kei), and gas prices are quoted in ston
* **Kakao and LINE Ecosystem**: Backed by two of Asia's largest platforms, with a focus on consumer and mini-dApp use cases

{% hint style="info" %}
_**TECHNICAL DISCLAIMER: AUTHORITATIVE JSON-RPC SPECIFICATION**_

_GetBlock's Kaia API reference documentation is provided exclusively for informational purposes and to optimize the developer experience. The canonical and normative specification for the Ethereum JSON-RPC API is maintained by the Ethereum community and published at_ [_ethereum.org/en/developers/docs/apis/json-rpc/_](https://ethereum.org/en/developers/docs/apis/json-rpc/)_. For the `kaia_` namespace and Kaia-specific protocol details, consult the official documentation at_ [_docs.kaia.io_](https://docs.kaia.io/)_._
{% endhint %}

## Network Information

| Property          | Value                                             |
| ----------------- | ------------------------------------------------- |
| Network Name      | Kaia Mainnet                                      |
| Stage             | Mainnet (live)                                    |
| Chain ID          | 8217 (`0x2019`)                                   |
| Native Currency   | KAIA                                              |
| Decimals          | 18 (base unit: kei; gas unit: ston)               |
| Consensus         | BFT with a governance council, immediate finality |
| Smart Contract VM | EVM (EVM-compatible)                              |
| Address Format    | Ethereum-style (`0x…`, 20 bytes)                  |
| Block Explorer    | [kaiascan.io](https://kaiascan.io/)               |
| Testnet           | Kairos (Chain ID 1001)                            |

## Base URL

```bash
https://go.getblock.io/<ACCESS-TOKEN>/
```

All Kaia JSON-RPC methods are called by sending a `POST` request to the base URL with a standard JSON-RPC 2.0 body. For a WebSocket connection, use the WebSocket scheme: `wss://go.getblock.io/<ACCESS-TOKEN>/`.

## Supported Networks

| Network | JSON-RPC | WSS | Frankfurt, Germany |
| ------- | -------- | --- | ------------------ |
| Mainnet | ✅        | ✅   | ✅                  |

Kairos Testnet (Chain ID `1001`) is the test environment for staging contracts before mainnet. Confirm testnet endpoint availability from the GetBlock dashboard.

## Quickstart

In this section, you will learn how to make your first call with either:

* **Axios** (JavaScript / Node.js)
* **Python** (Requests library)

Before you begin, you must have already installed [`npm`](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm) or [`yarn`](https://classic.yarnpkg.com/lang/en/docs/install) on your local machine (for the Axios example) or Python and pip (for the Python example).

{% tabs %}
{% tab title="JavaScript (Axios)" %}
{% stepper %}
{% step %}
#### Setup project

Create and initialize a new project:

```bash
mkdir kaia-api-quickstart
cd kaia-api-quickstart
npm init --yes
```
{% endstep %}

{% step %}
#### Install Axios

```bash
npm install axios
```
{% endstep %}

{% step %}
#### Create file

Create a new file named `index.js`. This is where you will make your first call.
{% endstep %}

{% step %}
#### Set ES module type

Set the ES module `"type": "module"` in your `package.json`.
{% endstep %}

{% step %}
#### Add code

Add the following code to `index.js`:

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

Replace `<ACCESS-TOKEN>` with your actual access token from GetBlock.
{% endstep %}

{% step %}
#### Run the script

```bash
node index.js
```

Expected output:

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x2019"
}
```

The `result` field is the Kaia Mainnet chain ID (`0x2019` = 8217 in decimal). This confirms you are connected to the correct network.
{% endstep %}
{% endstepper %}
{% endtab %}

{% tab title="Python (Requests)" %}
{% stepper %}
{% step %}
#### Setup the project directory

```bash
mkdir kaia-api-quickstart
cd kaia-api-quickstart
```
{% endstep %}

{% step %}
#### Create and activate a virtual environment

```bash
python -m venv venv
source venv/bin/activate

# On Windows, use venv\Scripts\activate
```
{% endstep %}

{% step %}
#### Install requests

```bash
pip install requests
```
{% endstep %}

{% step %}
#### Create script

Create a file called `main.py` with the following content:

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

Replace `<ACCESS-TOKEN>` with your actual access token from GetBlock.
{% endstep %}

{% step %}
#### Run the script

```bash
python main.py
```
{% endstep %}
{% endstepper %}
{% endtab %}
{% endtabs %}

## Available API Methods

Kaia exposes the standard Ethereum JSON-RPC method set plus native `kaia_` methods. All methods are POST-only and called against the base URL above.

### Block & Chain Information

| Method                                | Description                                                   |
| ------------------------------------- | ------------------------------------------------------------- |
| eth\_blockNumber                      | Returns the current latest block number                       |
| eth\_chainId                          | Returns the chain ID (`0x2019` = 8217 for Kaia Mainnet)       |
| eth\_getBlockByNumber                 | Returns block information by block number                     |
| eth\_getBlockByHash                   | Returns block information by block hash                       |
| eth\_getBlockTransactionCountByNumber | Returns the number of transactions in a block by block number |
| eth\_getBlockTransactionCountByHash   | Returns the number of transactions in a block by block hash   |
| eth\_getBlockReceipts                 | Returns all transaction receipts for a given block            |
| eth\_syncing                          | Returns sync status or `false` if the node is fully synced    |

### Account & State

| Method                   | Description                                            |
| ------------------------ | ------------------------------------------------------ |
| eth\_getBalance          | Returns the native KAIA balance of an account (in kei) |
| eth\_getCode             | Returns the contract bytecode at a given address       |
| eth\_getStorageAt        | Returns the value at a specific storage slot           |
| eth\_getTransactionCount | Returns the transaction count (nonce) for an account   |

### Transactions

| Method                                   | Description                                      |
| ---------------------------------------- | ------------------------------------------------ |
| eth\_sendRawTransaction                  | Broadcasts a signed transaction                  |
| eth\_getTransactionByHash                | Returns a transaction by its hash                |
| eth\_getTransactionByBlockHashAndIndex   | Returns a transaction by block hash and index    |
| eth\_getTransactionByBlockNumberAndIndex | Returns a transaction by block number and index  |
| eth\_getTransactionReceipt               | Returns the receipt for a transaction by hash    |
| eth\_call                                | Executes a read-only call against contract state |
| eth\_estimateGas                         | Estimates gas required for a transaction         |

### Gas & Fee Market

| Method                    | Description                                               |
| ------------------------- | --------------------------------------------------------- |
| eth\_gasPrice             | Returns the current gas price in kei                      |
| eth\_maxPriorityFeePerGas | Returns the suggested max priority fee per gas (EIP-1559) |
| eth\_feeHistory           | Returns historical base fees and priority fees            |

### Logs

| Method       | Description                          |
| ------------ | ------------------------------------ |
| eth\_getLogs | Returns logs matching a given filter |

### Debug & Trace

| Method                    | Description                                          |
| ------------------------- | ---------------------------------------------------- |
| debug\_traceTransaction   | Replays a transaction and returns an execution trace |
| debug\_traceBlockByNumber | Traces every transaction in a block by block number  |
| debug\_traceBlockByHash   | Traces every transaction in a block by block hash    |

### Kaia Native (`kaia_`)

| Method                                  | Description                                             |
| --------------------------------------- | ------------------------------------------------------- |
| kaia\_getAccount                        | Returns the Kaia account object, including type and key |
| kaia\_getAccountKey                     | Returns the account key of an EOA                       |
| kaia\_getBlockWithConsensusInfoByNumber | Returns a block with its proposer and committee         |
| kaia\_getCouncil                        | Returns the governance council validators at a block    |
| kaia\_getCommittee                      | Returns the committee validators for a block            |
| kaia\_getRewards                        | Returns the block reward distribution                   |
| kaia\_isContractAccount                 | Returns whether an address is a Smart Contract Account  |

### Network & Client Info

| Method              | Description                                      |
| ------------------- | ------------------------------------------------ |
| net\_version        | Returns the network ID (`8217` for Kaia Mainnet) |
| web3\_clientVersion | Returns the client software version              |

## Support

For technical support and questions:

* **Support Email**: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [Official Kaia Documentation](https://docs.kaia.io/)
* [Kaia JSON-RPC Reference](https://docs.kaia.io/references/json-rpc/)
* [Ethereum JSON-RPC Specification](https://ethereum.org/en/developers/docs/apis/json-rpc/)
* [Kaia Block Explorer (KaiaScan)](https://kaiascan.io/)
* [Kaia on Chainlist](https://chainlist.org/chain/8217)
* [Ethers.js Documentation](https://docs.ethers.org/)
* [Viem Documentation](https://viem.sh/)
