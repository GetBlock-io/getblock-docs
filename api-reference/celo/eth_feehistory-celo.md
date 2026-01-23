---
description: >-
  Example code for the eth_feeHistory JSON-RPC method. Complete guide on how to
  use eth_feeHistory JSON-RPC in GetBlock Web3 documentation.
---

# eth\_feeHistory - Celo

This method returns historical gas information on the Celo network, enabling accurate fee estimation for EIP-1559 style transactions. Celo maintains extremely low fees, typically under $0.01 per transaction.

## Parameters

| Parameter         | Type   | Required | Description                             |
| ----------------- | ------ | -------- | --------------------------------------- |
| blockCount        | string | Yes      | Number of blocks to return (hex)        |
| newestBlock       | string | Yes      | Newest block ("latest" or block number) |
| rewardPercentiles | array  | No       | Percentile values for priority fees     |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X POST https://go.getblock.io/<ACCESS-TOKEN>/ \
-H "Content-Type: application/json" \
-d '{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "eth_feeHistory",
  "params": ["0x5", "latest", [25, 50, 75]]
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
  method: 'eth_feeHistory',
  params: ['0x5', 'latest', [25, 50, 75]]
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
    "method": "eth_feeHistory",
    "params": ["0x5", "latest", [25, 50, 75]]
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
print(response.json())
```
{% endtab %}
{% endtabs %}

## Response Example

```json
{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": {
    "baseFeePerGas": ["0x5d21dba00", "0x5d21dba00", "0x5d21dba00"],
    "gasUsedRatio": [0.021, 0.045, 0.032],
    "oldestBlock": "0x1d9f2a4",
    "reward": [["0x0", "0x0", "0x0"], ["0x0", "0x0", "0x0"]]
  }
}
```

## Reponse Definition

| Field         | Type   | Description                     |
| ------------- | ------ | ------------------------------- |
| baseFeePerGas | array  | Base fee per gas for each block |
| gasUsedRatio  | array  | Gas used ratio for each block   |
| oldestBlock   | string | Oldest block number returned    |
| reward        | array  | Priority fee percentiles        |

## Use Cases

* Calculate optimal gas prices
* Implement EIP-1559 fee estimation
* Analyze network congestion
* Build fee prediction models

## Error Handling

| Error Code | Description    |
| ---------- | -------------- |
| -32602     | Invalid params |
| -32603     | Internal error |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const feeData = await provider.getFeeData();
console.log('Max Fee:', ethers.formatGwei(feeData.maxFeePerGas));
console.log('Priority Fee:', ethers.formatGwei(feeData.maxPriorityFeePerGas));
```
{% endtab %}
{% endtabs %}
