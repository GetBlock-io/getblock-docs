---
description: >-
  Example code for the eth_maxPriorityFeePerGas JSON-RPC method. Complete guide
  on how to use eth_maxPriorityFeePerGas JSON-RPC in GetBlock Web3
  documentation.
---

# eth\_maxPriorityFeePerGas - Celo

This method returns the current max priority fee per gas (tip) for EIP-1559 transactions on the Celo network. This is used to incentivize faster transaction inclusion.

### Parameters

* None

### Request Example

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

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
  jsonrpc: '2.0',
  id: 'getblock.io',
  method: 'eth_maxPriorityFeePerGas',
  params: []
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
    "method": "eth_maxPriorityFeePerGas",
    "params": []
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

### Response Example

{% code title="response.json" %}
```json
{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": "0x3b9aca00"
}
```
{% endcode %}

## Response Definition

| Field  | Type   | Description                            |
| ------ | ------ | -------------------------------------- |
| result | string | Max priority fee in wei as hexadecimal |

### Use Cases

* Set EIP-1559 transaction priority fees
* Optimize transaction inclusion speed
* Calculate total gas costs

### Error Handling

| Error Code | Description          |
| ---------- | -------------------- |
| -32603     | Internal error       |
| -32601     | Method not supported |

### SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const feeData = await provider.getFeeData();
console.log('Priority Fee:', ethers.formatGwei(feeData.maxPriorityFeePerGas), 'Gwei');
```
{% endcode %}
{% endtab %}
{% endtabs %}
