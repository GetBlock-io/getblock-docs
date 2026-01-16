---
description: >-
  Example code for the web3_sha3 JSON-RPC method. Complete guide on how to use
  web3_sha3 JSON-RPC in GetBlock Web3 documentation
---

# web3\_sha3 - Somnia

This method returns the Keccak-256 hash of the given data on the Somnia network. Note: This is Keccak-256, not the standardized SHA3-256.

## Parameters

| Parameter | Type   | Required | Description                |
| --------- | ------ | -------- | -------------------------- |
| data      | string | Yes      | Data to hash (hex encoded) |

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
  "method": "web3_sha3",
  "params": ["0x68656c6c6f"]
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
  method: 'web3_sha3',
  params: ['0x68656c6c6f']
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
    "method": "web3_sha3",
    "params": ["0x68656c6c6f"]
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
  "result": "0x1c8aff950685c2ed4bc3174f3472287b56d9517b9c948127319a09a7a36deac8"
}
```
{% endcode %}

## Use Cases

* Compute event topic signatures
* Generate function selectors
* Verify hashes
* Data integrity checks

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
const { ethers } = require('ethers');
const hash = ethers.keccak256('0x68656c6c6f');
console.log(hash);
```
{% endcode %}
{% endtab %}
{% endtabs %}
