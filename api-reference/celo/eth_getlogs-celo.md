---
description: >-
  Example code for the eth_getLogs JSON-RPC method. Complete guide on how to use
  eth_getLogs JSON-RPC in GetBlock Web3 documentation.
---

# eth\_getLogs - Celo

This method returns an array of all logs matching a given filter object on the Celo network. This is essential for tracking events emitted by smart contracts, monitoring stablecoin transfers (cUSD, cEUR), and building event-driven applications.

## Parameters

| Parameter    | Type   | Required | Description        |
| ------------ | ------ | -------- | ------------------ |
| filterObject | object | Yes      | The filter options |

### Filter Object

| Field     | Type         | Required | Description                                  |
| --------- | ------------ | -------- | -------------------------------------------- |
| fromBlock | string       | No       | Starting block (hex or "latest", "earliest") |
| toBlock   | string       | No       | Ending block (hex or "latest", "earliest")   |
| address   | string/array | No       | Contract address(es) to filter               |
| topics    | array        | No       | Array of topic filters                       |
| blockHash | string       | No       | Restrict to single block by hash             |

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
  "method": "eth_getLogs",
  "params": [
    {
      "fromBlock": "0x1d9f000",
      "toBlock": "latest",
      "address": "0x765DE816845861e75A25fCA122bb6898B8B1282a",
      "topics": ["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"]
    }
  ]
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code title="eth_getLogs.js" %}
```javascript
const axios = require('axios');

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

// Track cUSD Transfer events
const payload = {
  jsonrpc: '2.0',
  id: 'getblock.io',
  method: 'eth_getLogs',
  params: [
    {
      fromBlock: '0x1d9f000',
      toBlock: 'latest',
      address: '0x765DE816845861e75A25fCA122bb6898B8B1282a', // cUSD contract
      topics: ['0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef'] // Transfer event
    }
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
{% code title="eth_getLogs.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "eth_getLogs",
    "params": [
        {
            "fromBlock": "0x1d9f000",
            "toBlock": "latest",
            "address": "0x765DE816845861e75A25fCA122bb6898B8B1282a",
            "topics": ["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"]
        }
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
  "result": [
    {
      "address": "0x765DE816845861e75A25fCA122bb6898B8B1282a",
      "topics": [
        "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
        "0x000000000000000000000000742d35cc6634c0532925a3b844bc9e7595f8bb45",
        "0x0000000000000000000000008626f6940e2eb28930efb4cef49b2d1f2c9c1199"
      ],
      "data": "0x0000000000000000000000000000000000000000000000000de0b6b3a7640000",
      "blockNumber": "0x1d9f2a8",
      "transactionHash": "0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b",
      "transactionIndex": "0x0",
      "blockHash": "0x4e3a3754410177e6937ef1f84bba68ea139e8d1a2258c5f85db9f1cd715a1bdd",
      "logIndex": "0x0",
      "removed": false
    }
  ]
}
```
{% endcode %}

## Response Definition

| Field  | Type  | Description          |
| ------ | ----- | -------------------- |
| result | array | Array of log objects |

## Use Cases

* Track cUSD/cEUR stablecoin transfers
* Monitor ERC-20 token transfers
* Index DeFi protocol events
* Build event-driven applications
* Create analytics dashboards

## Error Handling

| Error Code | Description                             |
| ---------- | --------------------------------------- |
| -32602     | Invalid params - malformed filter       |
| -32603     | Internal error - node processing issues |
| -32005     | Query returned too many results         |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

// Track cUSD transfers
const logs = await provider.getLogs({
  fromBlock: 31000000,
  toBlock: 'latest',
  address: '0x765DE816845861e75A25fCA122bb6898B8B1282a',
  topics: ['0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef']
});
console.log(logs);
```
{% endcode %}
{% endtab %}

{% tab title="ContractKit" %}
{% code title="contractkit-example.js" %}
```javascript
const { newKit } = require('@celo/contractkit');

const kit = newKit('https://go.getblock.io/<ACCESS-TOKEN>/');

const logs = await kit.web3.eth.getPastLogs({
  fromBlock: 31000000,
  toBlock: 'latest',
  address: '0x765DE816845861e75A25fCA122bb6898B8B1282a',
  topics: ['0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef']
});
console.log(logs);
```
{% endcode %}
{% endtab %}
{% endtabs %}
