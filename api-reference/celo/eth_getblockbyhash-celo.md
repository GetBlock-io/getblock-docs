---
description: >-
  Example code for the eth_getBlockByHash JSON-RPC method. Complete guide on how
  to use eth_getBlockByHash JSON-RPC in GetBlock Web3 documentation.
---

# eth\_getBlockByHash - Celo

This method gets information about a block by its hash on the Celo network. This method is useful for retrieving specific block data when the block hash is known.

## Parameters

| Parameter        | Type    | Required | Description                                                                     |
| ---------------- | ------- | -------- | ------------------------------------------------------------------------------- |
| blockHash        | string  | Yes      | 32-byte block hash                                                              |
| fullTransactions | boolean | Yes      | If true, returns full transaction objects; if false, returns transaction hashes |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X POST https://go.getblock.io/<ACCESS-TOKEN>/ \
-H "Content-Type: application/json" \
-d '{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "eth_getBlockByHash",
  "params": [
    "0x4e3a3754410177e6937ef1f84bba68ea139e8d1a2258c5f85db9f1cd715a1bdd",
    false
  ]
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
const axios = require('axios');

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
  jsonrpc: '2.0',
  id: 'getblock.io',
  method: 'eth_getBlockByHash',
  params: [
    '0x4e3a3754410177e6937ef1f84bba68ea139e8d1a2258c5f85db9f1cd715a1bdd',
    false
  ]
};

axios.post(url, payload, {
  headers: { 'Content-Type': 'application/json' }
})
.then(response => console.log(response.data))
.catch(error => console.error(error));
```
{% endtab %}

{% tab title="Python" %}
{% code overflow="wrap" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "eth_getBlockByHash",
    "params": [
        "0x4e3a3754410177e6937ef1f84bba68ea139e8d1a2258c5f85db9f1cd715a1bdd",
        False
    ]
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
print(response.json())
```
{% endcode %}
{% endtab %}

{% tab title="Untitled" %}

{% endtab %}
{% endtabs %}

## Response Example

```json
{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": {
    "number": "0x1d9f2a8",
    "hash": "0x4e3a3754410177e6937ef1f84bba68ea139e8d1a2258c5f85db9f1cd715a1bdd",
    "parentHash": "0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238",
    "timestamp": "0x65a1b2c3",
    "gasLimit": "0x1c9c380",
    "gasUsed": "0x5208",
    "transactions": []
  }
}
```

## Response Definition

| Field        | Type   | Description                   |
| ------------ | ------ | ----------------------------- |
| number       | string | Block number                  |
| hash         | string | Block hash                    |
| parentHash   | string | Parent block hash             |
| timestamp    | string | Block timestamp               |
| gasLimit     | string | Maximum gas allowed           |
| gasUsed      | string | Total gas used                |
| transactions | array  | Transaction hashes or objects |

## Use Cases

* Retrieve specific block data by hash
* Verify block existence
* Build block explorers
* Track block confirmations

## Error Handling

| Error Code  | Description                             |
| ----------- | --------------------------------------- |
| -32602      | Invalid params - malformed block hash   |
| -32603      | Internal error - node processing issues |
| null result | Block not found                         |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code overflow="wrap" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const block = await provider.getBlock('0x4e3a3754410177e6937ef1f84bba68ea139e8d1a2258c5f85db9f1cd715a1bdd');
console.log(block);
```
{% endcode %}
{% endtab %}

{% tab title="ContractKit" %}
{% code overflow="wrap" %}
```javascript
const { newKit } = require('@celo/contractkit');

const kit = newKit('https://go.getblock.io/<ACCESS-TOKEN>/');

const block = await kit.web3.eth.getBlock('0x4e3a3754410177e6937ef1f84bba68ea139e8d1a2258c5f85db9f1cd715a1bdd');
console.log(block);
```
{% endcode %}
{% endtab %}
{% endtabs %}
