---
description: >-
  Example code for the eth_call JSON-RPC method. Complete guide on how to use
  eth_call JSON-RPC in GetBlock Web3 documentation.
---

# eth\_call - Celo

This method executes a call immediately without creating a transaction on Celo. Commonly used for reading data from smart contracts, simulating transactions, and querying contract state on the Celo network.

### Parameters

| Parameter   | Type   | Required | Description                                             |
| ----------- | ------ | -------- | ------------------------------------------------------- |
| transaction | object | Yes      | Transaction call object                                 |
| blockNumber | string | Yes      | Block number in hex, or "latest", "earliest", "pending" |

#### Transaction Object

| Field    | Type   | Required | Description           |
| -------- | ------ | -------- | --------------------- |
| from     | string | No       | Sender address        |
| to       | string | Yes      | Contract address      |
| gas      | string | No       | Gas limit             |
| gasPrice | string | No       | Gas price             |
| value    | string | No       | Value to send         |
| data     | string | No       | Encoded function call |

### Request Example

{% tabs %}
{% tab title="cURL" %}
{% code title="curl" overflow="wrap" %}
```bash
curl -X POST https://go.getblock.io/<ACCESS-TOKEN>/ \
-H "Content-Type: application/json" \
-d '{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "eth_call",
  "params": [
    {
      "to": "0x765DE816845861e75A25fCA122bb6898B8B1282a",
      "data": "0x70a08231000000000000000000000000742d35cc6634c0532925a3b844bc9e7595f8bb45"
    },
    "latest"
  ]
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
  method: 'eth_call',
  params: [
    {
      to: '0x765DE816845861e75A25fCA122bb6898B8B1282a', // cUSD contract
      data: '0x70a08231000000000000000000000000742d35cc6634c0532925a3b844bc9e7595f8bb45'
    },
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
{% code title="example.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "eth_call",
    "params": [
        {
            "to": "0x765DE816845861e75A25fCA122bb6898B8B1282a",
            "data": "0x70a08231000000000000000000000000742d35cc6634c0532925a3b844bc9e7595f8bb45"
        },
        "latest"
    ]
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
  "result": "0x0000000000000000000000000000000000000000000000000de0b6b3a7640000"
}
```
{% endcode %}

### Response Definition

| Field  | Type   | Description               |
| ------ | ------ | ------------------------- |
| result | string | Return data from the call |

### Use Cases

* Read cUSD/cEUR stablecoin balances
* Query contract state
* Simulate transactions before sending
* Get NFT metadata
* Check allowances and approvals

### Error Handling

| Error Code | Description                            |
| ---------- | -------------------------------------- |
| -32602     | Invalid params - malformed call object |
| -32603     | Internal error - execution reverted    |
| -32000     | Execution error - contract revert      |

### SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers.js example" overflow="wrap" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

// Read cUSD balance
const cusdAbi = ['function balanceOf(address) view returns (uint256)'];
const cusdContract = new ethers.Contract('0x765DE816845861e75A25fCA122bb6898B8B1282a', cusdAbi, provider);

const balance = await cusdContract.balanceOf('0x742d35Cc6634C0532925a3b844Bc9e7595f8bB45');
console.log(ethers.formatEther(balance));
```
{% endcode %}
{% endtab %}

{% tab title="ContractKit" %}
{% code title="contractkit example" overflow="wrap" %}
```javascript
const { newKit } = require('@celo/contractkit');

const kit = newKit('https://go.getblock.io/<ACCESS-TOKEN>/');

// Get cUSD contract
const stableToken = await kit.contracts.getStableToken();
const balance = await stableToken.balanceOf('0x742d35Cc6634C0532925a3b844Bc9e7595f8bB45');
console.log(balance.toString());
```
{% endcode %}
{% endtab %}
{% endtabs %}
