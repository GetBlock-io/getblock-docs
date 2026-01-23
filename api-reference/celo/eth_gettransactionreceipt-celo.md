---
description: >-
  Example code for the eth_getTransactionReceipt JSON-RPC method. Complete guide
  on how to use eth_getTransactionReceipt JSON-RPC in GetBlock Web3
  documentation.
---

# eth\_getTransactionReceipt - Celo

This method returns the receipt of a transaction by transaction hash on the Celo network. The receipt contains information about the execution result, including status, gas used, and logs emitted. This is essential for confirming transaction success and debugging.

## Parameters

| Parameter       | Type   | Required | Description              |
| --------------- | ------ | -------- | ------------------------ |
| transactionHash | string | Yes      | 32-byte transaction hash |

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
  "method": "eth_getTransactionReceipt",
  "params": ["0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"]
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
  method: 'eth_getTransactionReceipt',
  params: ['0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b']
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
    "method": "eth_getTransactionReceipt",
    "params": ["0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"]
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Definition

| Field           | Type   | Description                    |
| --------------- | ------ | ------------------------------ |
| transactionHash | string | Transaction hash               |
| status          | string | 0x1 (success) or 0x0 (failure) |
| blockHash       | string | Block hash                     |
| blockNumber     | string | Block number                   |
| gasUsed         | string | Gas used by transaction        |
| logs            | array  | Array of log objects           |
| contractAddress | string | Contract address if deployment |

## Response Example

{% code title="response.json" %}
```json
{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": {
    "transactionHash": "0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b",
    "transactionIndex": "0x0",
    "blockHash": "0x4e3a3754410177e6937ef1f84bba68ea139e8d1a2258c5f85db9f1cd715a1bdd",
    "blockNumber": "0x1d9f2a8",
    "from": "0x742d35Cc6634C0532925a3b844Bc9e7595f8bB45",
    "to": "0x8626f6940E2eb28930eFb4CeF49B2d1F2C9C1199",
    "cumulativeGasUsed": "0x5208",
    "gasUsed": "0x5208",
    "contractAddress": null,
    "logs": [],
    "status": "0x1",
    "effectiveGasPrice": "0x5d21dba00"
  }
}
```
{% endcode %}

## Use Cases

* Confirm transaction success or failure
* Get deployed contract addresses
* Parse emitted events and logs
* Calculate actual transaction costs
* Debug reverted transactions

## Error Handling

| Error Code  | Description                             |
| ----------- | --------------------------------------- |
| -32602      | Invalid params - malformed hash         |
| -32603      | Internal error - node processing issues |
| null result | Transaction not found or pending        |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers.js" overflow="wrap" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const receipt = await provider.getTransactionReceipt('0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b');
console.log(receipt);
```
{% endcode %}
{% endtab %}

{% tab title="ContractKit" %}
{% code title="contractkit.js" %}
```javascript
const { newKit } = require('@celo/contractkit');

const kit = newKit('https://go.getblock.io/<ACCESS-TOKEN>/');

const receipt = await kit.web3.eth.getTransactionReceipt('0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b');
console.log(receipt);
```
{% endcode %}
{% endtab %}
{% endtabs %}
