# Ethereum Classic (ETC)

Ethereum Classic is the continuation of the original Ethereum chain, secured by Proof-of-Work (the Etchash algorithm) and governed by a strict code-is-law immutability ethos. It is EVM-compatible at the Spiral (Shanghai) opcode level, so standard Ethereum contracts, the `eth_*` JSON-RPC surface, and tooling such as Foundry, Hardhat, Ethers.js, and Viem work unchanged — with one important difference: Ethereum Classic did not adopt EIP-1559, so it uses legacy gas pricing with no base fee. The native token is ETC. Nodes (Core-Geth) expose three interfaces: JSON-RPC over HTTP, WebSocket, and an EIP-1767 GraphQL API.

### Key Features

* **EVM Compatibility**: Standard Ethereum contracts, JSON-RPC, and tooling run unchanged (Spiral / Shanghai opcodes)
* **Proof-of-Work**: The only major EVM chain secured by Proof-of-Work (Etchash), with GPU and ASIC mining
* **Legacy Gas**: No EIP-1559 base fee; transactions use `gasPrice`, and PoW mining methods are available
* **Immutability**: A code-is-law chain that does not perform state-reverting forks
* **Three Interfaces**: JSON-RPC (HTTP), WebSocket (subscriptions), and an EIP-1767 GraphQL API
* **Ethereum Tooling**: Compatible with MetaMask, Foundry, Hardhat, Remix, Ethers.js, and Viem

{% hint style="info" %}
_TECHNICAL DISCLAIMER: AUTHORITATIVE JSON-RPC API SPECIFICATION._

_GetBlock's RPC API reference documentation is provided exclusively for informational purposes and to optimize the developer experience. Ethereum Classic implements the standard Ethereum JSON-RPC interface; the canonical specification is the Ethereum JSON-RPC specification at_ [_ethereum.org_](https://ethereum.org/en/developers/docs/apis/json-rpc/)_, and Ethereum Classic protocol specifications (ECIPs) are published at_ [_ethereumclassic.org_](https://ethereumclassic.org/)_._
{% endhint %}

## Network Information

| Property        | Value                             |
| --------------- | --------------------------------- |
| Network Name    | Ethereum Classic                  |
| Chain ID        | 61                                |
| Native Currency | ETC                               |
| EVM Compatible  | Yes (Spiral / Shanghai)           |
| Consensus       | Proof-of-Work (Etchash)           |
| Block Time      | \~13 seconds                      |
| Finality        | Probabilistic (use confirmations) |
| Gas Pricing     | Legacy (no EIP-1559 base fee)     |

## Available Interfaces

Ethereum Classic nodes expose the same chain over three interfaces. GetBlock serves all three; each is provisioned as its own endpoint.

| Interface       | Transport                            | Use it for                                                                                |
| --------------- | ------------------------------------ | ----------------------------------------------------------------------------------------- |
| JSON-RPC (HTTP) | Ethereum JSON-RPC 2.0 over HTTP      | `eth_*` / `net_*` / `web3_*` request-response calls (documented here)                     |
| WebSocket (WSS) | Ethereum JSON-RPC 2.0 over WebSocket | The same methods plus `eth_subscribe` streams (newHeads, logs, newPendingTransactions)    |
| GraphQL         | EIP-1767 GraphQL over HTTP           | Typed, flexible queries over blocks, transactions, accounts, and logs in a single request |

The JSON-RPC and WebSocket interfaces carry the same `eth_*` method set (WebSocket adds subscriptions); the [GraphQL API](graphql-api/) is a separate query interface documented alongside this one.

## Base URL

{% tabs %}
{% tab title="Frankfurt, Germany" %}
```
https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}

{% tab title="New York, USA" %}
```
https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}

{% tab title="Singapore, Singapore" %}
```
https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}
{% endtabs %}

## Supported Networks

| Network | Chain ID | JSON-RPC | WSS | GraphQL | MEV protected (WebSocket) | MEV protected (JSON-RPC) | Frankfurt, Germany | New York, USA | Singapore, Singapore |
| ------- | -------- | -------- | --- | ------- | ------------------------- | ------------------------ | ------------------ | ------------- | -------------------- |
| Mainnet | 61       | ✅        | ✅   | ✅       | ❌                         | ❌                        | ✅                  | ❌             | ❌                    |
| Mordor  | 63       | ✅        | ✅   | ✅       | ❌                         | ❌                        | ✅                  | ❌             | ❌                    |

## Quickstart

{% tabs %}
{% tab title="Javascript(Axios)" %}
{% stepper %}
{% step %}
### Setup project

```bash
mkdir etc-api-quickstart && cd etc-api-quickstart && npm init --yes
```
{% endstep %}

{% step %}
### Install dependency

```bash
npm install axios
```
{% endstep %}

{% step %}
### Add code

{% code title="index.js" %}
```javascript
const axios = require('axios');

const url = 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/';

const payload = {
  jsonrpc: '2.0',
  method: 'eth_blockNumber',
  params: [],
  id: 'getblock.io'
};

axios.post(url, payload, {
  headers: { 'Content-Type': 'application/json' }
})
.then(response => {
  console.log('Latest block:', parseInt(response.data.result, 16));
})
.catch(error => console.error(error));
```
{% endcode %}
{% endstep %}

{% step %}
### Run the script

```bash
node index.js
```
{% endstep %}

{% step %}
### Response

{% code overflow="wrap" %}
```bash
{
    "jsonrpc": "2.0",
    "result": "0x18201de",
    "id": "getblock.io"
}
```
{% endcode %}
{% endstep %}
{% endstepper %}
{% endtab %}

{% tab title="Python(Request)" %}
{% stepper %}
{% step %}
### Setup project

```bash
mkdir etc-api-quickstart && cd etc-api-quickstart
python -m venv venv && source venv/bin/activate
```
{% endstep %}

{% step %}
### Install dependency

```bash
pip install requests
```
{% endstep %}

{% step %}
### Add code

{% code title="main.py" %}
```python
import requests
import json

url = "https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "eth_blockNumber",
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

## APIs

* [GraphQL API](graphql-api/) — the EIP-1767 GraphQL query interface
* [JSON RPC API ](json-rpc-api/)

## Support

For technical support and questions:

* Support: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [Ethereum Classic](https://ethereumclassic.org/)
* [Ethereum JSON-RPC Specification](https://ethereum.org/en/developers/docs/apis/json-rpc/)
* [Ethereum GraphQL (EIP-1767)](https://eips.ethereum.org/EIPS/eip-1767)
* [Blockscout Explorer (ETC)](https://etc.blockscout.com/)
