---
description: >-
  Example code for the eth_getTransactionCount JSON-RPC method. Complete guide
  on how to use eth_getTransactionCount JSON-RPC in GetBlock Web3 documentation.
---

# eth\_getTransactionCount - Celo

This method returns the number of transactions sent from an address on the Celo network. This value is also known as the nonce and is essential for transaction signing and preventing replay attacks.

## Parameters

| Parameter   | Type   | Required | Description                                             |
| ----------- | ------ | -------- | ------------------------------------------------------- |
| address     | string | Yes      | The address to get transaction count for                |
| blockNumber | string | Yes      | Block number in hex, or "latest", "earliest", "pending" |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
```bash
curl -X POST https://go.getblock.io/<ACCESS-TOKEN>/ \
-H "Content-Type: application/json" \
-d '{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "eth_getTransactionCount",
  "params": [
    "0x742d35Cc6634C0532925a3b844Bc9e7595f8bB45",
    "latest"
  ]
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code title="request.js" %}
```javascript
const axios = require('axios');

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
  jsonrpc: '2.0',
  id: 'getblock.io',
  method: 'eth_getTransactionCount',
  params: [
    '0x742d35Cc6634C0532925a3b844Bc9e7595f8bB45',
    'latest'
  ]
};

axios.post(url, payload, {
  headers: { 'Content-Type': 'application/json' }
})
.then(response => console.log(response.data))
.catch(error => console.error(error));
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="request.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "eth_getTransactionCount",
    "params": [
        "0x742d35Cc6634C0532925a3b844Bc9e7595f8bB45",
        "latest"
    ]
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Example

```json
{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": "0x15"
}
```

## Response Definition

| Field  | Type   | Description                              |
| ------ | ------ | ---------------------------------------- |
| result | string | Transaction count (nonce) as hexadecimal |

## Use Cases

* Get nonce for transaction signing
* Track account activity
* Detect pending transactions
* Prevent nonce conflicts

## Error Handling

| Error Code | Description                             |
| ---------- | --------------------------------------- |
| -32602     | Invalid params - malformed address      |
| -32603     | Internal error - node processing issues |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers.js" overflow="wrap" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const nonce = await provider.getTransactionCount('0x742d35Cc6634C0532925a3b844Bc9e7595f8bB45');
console.log(nonce);
```
{% endcode %}
{% endtab %}

{% tab title="ContractKit" %}
{% code title="contractkit.js" %}
```javascript
const { newKit } = require('@celo/contractkit');

const kit = newKit('https://go.getblock.io/<ACCESS-TOKEN>/');

const nonce = await kit.web3.eth.getTransactionCount('0x742d35Cc6634C0532925a3b844Bc9e7595f8bB45');
console.log(nonce);
```
{% endcode %}
{% endtab %}
{% endtabs %}
