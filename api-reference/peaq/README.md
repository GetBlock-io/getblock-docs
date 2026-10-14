---
description: >-
  GetBlock provides fast and reliable access to Peaq nodes via JSON-RPC API.
  Connect to the Peaq network without running your own infrastructure.
---

# Peaq

Peaq is a Substrate/Polkadot Layer-1 SDK for DePIN (Decentralized Physical Infrastructure Networks) and the machine economy. Its node exposes a rich JSON-RPC surface over a single endpoint: the classic Substrate method families (`system_`, `chain_`, `state_`, `childstate_`, `author_`, `payment_`, `offchain_`, `rpc_methods`), the newer JSON-RPC-spec families (`chainHead_v1_`, `chainSpec_v1_`, `transaction_v1_`, `transactionWatch_v1_`), a full Ethereum-compatible EVM layer via Frontier (`eth_`, `net_`, `web3_`), peaq's own machine-economy pallets (`peaqdid_`, `peaqrbac_`, `peaqstorage_`), and the Zenlink DEX (`zenlinkProtocol_`). The native token is PEAQ (18 decimals; SS58 prefix 42).

## Interfaces

| Interface       | Transport                   | Use it for                                                                                                       |
| --------------- | --------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| JSON-RPC (HTTP) | JSON-RPC 2.0 over HTTP POST | All request/response methods — Substrate, EVM, peaq pallets, and Zenlink                                         |
| WebSocket (WSS) | JSON-RPC 2.0 over WebSocket | The same methods, plus pub-sub subscriptions (new heads, storage, extrinsic/transaction status, `eth_subscribe`) |

Both interfaces carry the same method set; subscription methods (marked below) require WebSocket. Every call is a POST with a `method` and a `params` array to the single endpoint.

## Interface Endpoints

{% tabs %}
{% tab title="JSON-RPC (HTTP)" %}
```bash
https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}

{% tab title="WebSocket (WSS)" %}
```bash
wss://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Replace `<ACCESS-TOKEN>` with the token from the GetBlock dashboard. The peaq node serves the Substrate and EVM method families on the same endpoint; subscription methods require the WebSocket interface.
{% endhint %}

## Network Information

<table data-search="false"><thead><tr><th>Property</th><th>Value</th></tr></thead><tbody><tr><td>Network Name</td><td>peaq</td></tr><tr><td>Native Currency</td><td>PEAQ (18 decimals)</td></tr><tr><td>SS58 Format</td><td>42</td></tr><tr><td>EVM Chain ID</td><td>3338</td></tr><tr><td>Framework</td><td>Substrate / Polkadot SDK (Layer-1) with Frontier EVM</td></tr><tr><td>Consensus</td><td>Proof-of-Stake (Aura block production + GRANDPA finality)</td></tr><tr><td>Finality</td><td>Deterministic (GRANDPA)</td></tr></tbody></table>

## Supported Networks

| Network | JSON-RPC (HTTP) | WebSocket (WSS) | Frankfurt, Germany | New York, USA | Singapore, Singapore |
| ------- | --------------- | --------------- | ------------------ | ------------- | -------------------- |
| Mainnet | ✅               | ✅               | ❌                  | ✅             | ❌                    |

## Quickstart

{% tabs %}
{% tab title="Javascript(Axios)" %}
{% stepper %}
{% step %}
### Setup project

```bash
mkdir peaq-api-quickstart && cd peaq-api-quickstart  && npm init --yes
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

const url = 'https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/';

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
Latest block: 11,979,753
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
mkdir peaq-api-quickstart && cd peaq-api-quickstart
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

url = "https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/"

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

## Support

For technical support and questions:

* Support: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [peaq Documentation](https://docs.peaq.xyz/)
* [Substrate JSON-RPC](https://docs.substrate.io/)
* [Polkadot-JS API](https://polkadot.js.org/docs/)
* [Subscan Explorer (peaq)](https://peaq.subscan.io/)
