---
description: >-
  Example code for the web3_sha3 JSON-RPC method. Complete guide on how to use
  web3_sha3 JSON-RPC in GetBlock Web3 documentation.
---

# web3\_sha3 - Celo

This method the Keccak-256 hash of the given data on Celo. This is useful for hashing data for signatures, event topic generation, and other cryptographic operations.

## Parameters

| Parameter | Type   | Required | Description                |
| --------- | ------ | -------- | -------------------------- |
| data      | string | Yes      | Data to hash (hex encoded) |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code title="curl" %}
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

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
  jsonrpc: '2.0',
  id: 'getblock.io',
  method: 'web3_sha3',
  params: ['0x68656c6c6f'] // "hello" in hex
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
    "method": "web3_sha3",
    "params": ["0x68656c6c6f"]
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
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

## Response Definition

| Field  | Type   | Description     |
| ------ | ------ | --------------- |
| result | string | Keccak-256 hash |

## Use Cases

* Generate event signatures
* Hash data for verification
* Create function selectors
* Cryptographic operations

## Error Handling

| Error Code | Description    |
| ---------- | -------------- |
| -32602     | Invalid params |
| -32603     | Internal error |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers.js" %}
```javascript
const { ethers } = require('ethers');

const hash = ethers.keccak256(ethers.toUtf8Bytes('hello'));
console.log(hash);
```
{% endcode %}
{% endtab %}
{% endtabs %}
