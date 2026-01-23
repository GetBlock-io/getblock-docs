---
description: >-
  Example code for the eth_getCode JSON-RPC method. Complete guide on how to use
  eth_getCode JSON-RPC in GetBlock Web3 documentation.
---

# eth\_getCode - Celo

This method returns the bytecode stored at a given address on the Celo network. This is useful for verifying contract deployments and distinguishing between EOAs (Externally Owned Accounts) and contract accounts.

## Parameters

| Parameter   | Type   | Required | Description                                             |
| ----------- | ------ | -------- | ------------------------------------------------------- |
| address     | string | Yes      | Contract address to query                               |
| blockNumber | string | Yes      | Block number in hex, or "latest", "earliest", "pending" |

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
  "method": "eth_getCode",
  "params": [
    "0x765DE816845861e75A25fCA122bb6898B8B1282a",
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

// Query cUSD contract bytecode
const payload = {
  jsonrpc: '2.0',
  id: 'getblock.io',
  method: 'eth_getCode',
  params: [
    '0x765DE816845861e75A25fCA122bb6898B8B1282a', // cUSD contract
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
    "method": "eth_getCode",
    "params": [
        "0x765DE816845861e75A25fCA122bb6898B8B1282a",
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

{% code title="response.json" %}
```json
{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": "0x608060405234801561001057600080fd5b50600436106100..."
}
```
{% endcode %}

## Response Definition

| Field  | Type   | Description                     |
| ------ | ------ | ------------------------------- |
| result | string | Contract bytecode as hex string |

## Use Cases

* Verify contract deployment
* Check if address is a contract or EOA
* Analyze contract bytecode
* Validate proxy implementations

## Error Handling

| Error Code | Description                             |
| ---------- | --------------------------------------- |
| -32602     | Invalid params - malformed address      |
| -32603     | Internal error - node processing issues |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const code = await provider.getCode('0x765DE816845861e75A25fCA122bb6898B8B1282a');
console.log(code);
console.log('Is contract:', code !== '0x');
```
{% endcode %}
{% endtab %}

{% tab title="ContractKit" %}
{% code title="contractkit.js" %}
```javascript
const { newKit } = require('@celo/contractkit');

const kit = newKit('https://go.getblock.io/<ACCESS-TOKEN>/');

const code = await kit.web3.eth.getCode('0x765DE816845861e75A25fCA122bb6898B8B1282a');
console.log(code);
```
{% endcode %}
{% endtab %}
{% endtabs %}
