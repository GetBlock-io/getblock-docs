---
description: >-
  Example code for the eth_maxPriorityFeePerGas JSON-RPC method. Complete guide
  on how to use eth_maxPriorityFeePerGas JSON-RPC in GetBlock Web3
  documentation.
---

# eth\_maxPriorityFeePerGas - Somnia

This method returns the current max priority fee per gas on the Somnia network (EIP-1559). This helps set appropriate priority fees for faster transaction inclusion.

## Parameters

* None

## Returns

| Field  | Type   | Description                   |
| ------ | ------ | ----------------------------- |
| result | string | Max priority fee in wei (hex) |

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
  "method": "eth_maxPriorityFeePerGas",
  "params": []
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code title="request.js" %}
```javascript
const axios = require('axios');

const payload = {
  jsonrpc: '2.0',
  id: 'getblock.io',
  method: 'eth_maxPriorityFeePerGas',
  params: []
};

axios.post('https://go.getblock.io/<ACCESS-TOKEN>/', payload)
.then(response => console.log(response.data));
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="request.py" %}
```python
import requests

payload = {
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "eth_maxPriorityFeePerGas",
    "params": []
}

response = requests.post("https://go.getblock.io/<ACCESS-TOKEN>/", json=payload)
print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Example

{% code title="response.json" %}
```json
{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": "0x3b9aca00"
}
```
{% endcode %}

## Use Cases

* Set EIP-1559 transaction fees
* Optimize gas pricing
* Priority transaction submission

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers.js" %}
```javascript
const { ethers } = require('ethers');
const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');
const feeData = await provider.getFeeData();
console.log(feeData.maxPriorityFeePerGas);
```
{% endcode %}
{% endtab %}
{% endtabs %}
