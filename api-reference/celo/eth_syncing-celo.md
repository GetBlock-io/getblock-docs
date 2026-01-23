---
description: >-
  Example code for the eth_syncing JSON-RPC method. Complete guide on how to use
  eth_syncing JSON-RPC in GetBlock Web3 documentation.
---

# eth\_syncing - Celo

The `eth_syncing` method returns an object with data about the sync status of the Celo node, or `false` if the node is fully synced. This is useful for monitoring node health and readiness.

## Parameters

* None

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X POST https://go.getblock.io/<ACCESS-TOKEN>/ \
-H "Content-Type: application/json" \
-d '{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "eth_syncing",
  "params": []
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
  method: 'eth_syncing',
  params: []
};

axios.post(url, payload, {
  headers: { 'Content-Type': 'application/json' }
})
.then(response => console.log(response.data))
.catch(error => console.error(error));
```
{% endtab %}

{% tab title="Python" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "eth_syncing",
    "params": []
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
print(response.json())
```
{% endtab %}
{% endtabs %}

## Response Example

{% tabs %}
{% tab title="Synced" %}
```json
{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": false
}
```
{% endtab %}

{% tab title="Syncing" %}
```json
{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": {
    "startingBlock": "0x0",
    "currentBlock": "0x1d9f000",
    "highestBlock": "0x1d9f2a8"
  }
}
```
{% endtab %}
{% endtabs %}

## Response Definition

{% tabs %}
{% tab title="Synced" %}
| Field  | Type           | Description                              |
| ------ | -------------- | ---------------------------------------- |
| result | boolean/object | `false` if synced, or sync status object |
{% endtab %}

{% tab title="Syncing" %}
| Field         | Type   | Description                   |
| ------------- | ------ | ----------------------------- |
| startingBlock | string | Block at which sync started   |
| currentBlock  | string | Current block being processed |
| highestBlock  | string | Highest known block           |
{% endtab %}
{% endtabs %}

## Use Cases

* Monitor node synchronization
* Check node health before sending transactions
* Implement health checks in applications

## Error Handling

| Error Code | Description    |
| ---------- | -------------- |
| -32603     | Internal error |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const syncStatus = await provider.send('eth_syncing', []);
console.log('Synced:', syncStatus === false);
```
{% endtab %}

{% tab title="ContractKit" %}
```javascript
const { newKit } = require('@celo/contractkit');

const kit = newKit('https://go.getblock.io/<ACCESS-TOKEN>/');

const syncing = await kit.web3.eth.isSyncing();
console.log('Syncing:', syncing);
```
{% endtab %}
{% endtabs %}
