---
description: >-
  Example code for the eth_newFilter JSON-RPC method. Complete guide on how to
  use eth_newFilter JSON-RPC in GetBlock Web3 documentation.
---

# eth\_newFilter - Celo

This method creates a filter object for logs on the Celo network, which can be polled with `eth_getFilterChanges`. This is useful for tracking specific events like cUSD transfers or DeFi protocol activity.

## Parameters

| Parameter    | Type   | Required | Description    |
| ------------ | ------ | -------- | -------------- |
| filterObject | object | Yes      | Filter options |

### Filter Object

| Field     | Type         | Required | Description          |
| --------- | ------------ | -------- | -------------------- |
| fromBlock | string       | No       | Starting block       |
| toBlock   | string       | No       | Ending block         |
| address   | string/array | No       | Contract address(es) |
| topics    | array        | No       | Event topics         |

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
  "method": "eth_newFilter",
  "params": [{
    "fromBlock": "latest",
    "address": "0x765DE816845861e75A25fCA122bb6898B8B1282a",
    "topics": ["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"]
  }]
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
  jsonrpc: '2.0',
  id: 'getblock.io',
  method: 'eth_newFilter',
  params: [{
    fromBlock: 'latest',
    address: '0x765DE816845861e75A25fCA122bb6898B8B1282a',
    topics: ['0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef']
  }]
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
{% code title="example.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "eth_newFilter",
    "params": [{
        "fromBlock": "latest",
        "address": "0x765DE816845861e75A25fCA122bb6898B8B1282a",
        "topics": ["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"]
    }]
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
  "result": "0x1"
}
```
{% endcode %}

## Response Definition

| Field  | Type   | Description |
| ------ | ------ | ----------- |
| result | string | Filter ID   |

## Use Cases

* Track stablecoin transfers (cUSD, cEUR)
* Monitor DeFi events
* Build event notification systems
* Index contract events

## Error Handling

| Error Code | Description    |
| ---------- | -------------- |
| -32602     | Invalid params |
| -32603     | Internal error |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const filter = {
  address: '0x765DE816845861e75A25fCA122bb6898B8B1282a',
  topics: ['0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef']
};

provider.on(filter, (log) => {
  console.log('Transfer event:', log);
});
```
{% endcode %}
{% endtab %}
{% endtabs %}
