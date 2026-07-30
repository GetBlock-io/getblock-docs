---
description: >-
  Polygon Network API Reference for efficient interaction with MATIC nodes,
  enabling fast and low-cost transactions with enhanced scalability on
  Ethereum-compatible blockchains.
---

# Polygon (MATIC)

Polygon Network API Reference for efficient interaction with Polygon PoS nodes, enabling fast and low-cost transactions with enhanced scalability on Ethereum-compatible blockchains.

Polygon (formerly Matic Network) is a Layer 2 scaling solution for Ethereum that provides faster and cheaper transactions while maintaining security through its Proof-of-Stake consensus mechanism. The network is fully EVM-compatible, meaning developers can use the same Ethereum tools, libraries, and JSON-RPC methods to interact with Polygon.

This API reference provides comprehensive documentation for all available JSON-RPC methods supported by GetBlock's Polygon infrastructure. These methods enable developers to query blockchain data, submit transactions, interact with smart contracts, and build decentralized applications.

{% hint style="info" %}
_TECHNICAL DISCLAIMER: AUTHORITATIVE JSON-RPC API SPECIFICATION._&#x20;

_GetBlock's RPC API reference documentation is provided exclusively for informational purposes and to optimize the developer experience. The canonical and normative specification for Ethereum Virtual Machine (EVM) JSON-RPC methods is solely maintained and published through the official Ethereum Foundation documentation portal at_ [_ethereum.org_](http://ethereum.org/)_. This resource constitutes the sole authoritative reference implementation of the JSON-RPC 2.0 protocol interface across EVM-compatible execution clients._
{% endhint %}

## Network Information

| Property        | Value                |
| --------------- | -------------------- |
| Network Name    | Polygon Chain        |
| Chain ID        | 137                  |
| Native Currency | POL                  |
| Decimals        | 18                   |
| Block Time      | \~2 seconds          |
| Consensus       | Proof-of-Stake (PoS) |
| EVM Compatible  | Yes                  |

## Base URL

{% tabs %}
{% tab title="Frankfurt, Germany" %}
```
https://go.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}

{% tab title="New York, USA" %}
```
https://go.getblock.us/<ACCESS-TOKEN>/
```
{% endtab %}

{% tab title="Singapore, Singapore" %}
```
https://go.getblock.asia/<ACCESS-TOKEN>/
```
{% endtab %}
{% endtabs %}

## Supported Networks

| Network | Chain ID | JSON-RPC | WSS | gRPC | Frankfurt, Germany | New York, USA | Singapore, Singapore |
| ------- | -------- | -------- | --- | ---- | ------------------ | ------------- | -------------------- |
| Mainnet | 137      | ✅        | ✅   | ✅    | ✅                  | ✅             | ✅                    |
| Amoy    | 80002    | ✅        | ✅   | ✅    | ✅                  | ✅             | ❌                    |

## Quickstart

{% tabs %}
{% tab title="Javascript(Axios)" %}
{% stepper %}
{% step %}
### Setup project

Create and initialize a new project:

```bash
mkdir polygon-api-quickstart
cd polygon-api-quickstart
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
const axios = require('axios');

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

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
  const blockNumber = parseInt(response.data.result, 16);
  console.log('Current Block Number:', blockNumber);
})
.catch(error => console.error(error));
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
    "id": "getblock.io",
    "result": "0x4a87a53"
}
```
{% endstep %}
{% endstepper %}
{% endtab %}

{% tab title="Python(Request)" %}
{% stepper %}
{% step %}
### Set up the project directory

```bash
mkdir polygon-api-quickstart
cd polygon-api-quickstart
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

## API Categories



## Support

For technical support and questions:

* Support: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [Polygon Documentation](https://docs.polygon.technology/)
* [Polygon PoS Block Explorer](https://polygonscan.com/)
* [Polygon Website](https://polygon.technology/)
* [Ethereum JSON-RPC Specification](https://ethereum.org/developers/docs/apis/json-rpc/)
