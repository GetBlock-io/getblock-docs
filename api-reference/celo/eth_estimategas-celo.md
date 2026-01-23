---
description: >-
  Example code for the eth_estimateGas JSON-RPC method. Complete guide on how to
  use eth_estimateGas JSON-RPC in GetBlock Web3 documentation.
---

# eth\_estimateGas - Celo

This method generates an estimate of how much gas is necessary to complete a transaction on the Celo network. This is essential for setting appropriate gas limits. Celo's low gas costs make accurate estimation important for optimal fee management.

{% hint style="info" %}
Use eth\_estimateGas to calculate gas requirements before sending transactions to avoid out-of-gas errors and set appropriate gas limits.
{% endhint %}

## Parameters

| Parameter   | Type   | Required | Description                                |
| ----------- | ------ | -------- | ------------------------------------------ |
| transaction | object | Yes      | Transaction call object                    |
| blockNumber | string | No       | Block number in hex, or "latest" (default) |

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
  "method": "eth_estimateGas",
  "params": [
    {
      "from": "0x742d35Cc6634C0532925a3b844Bc9e7595f8bB45",
      "to": "0x8626f6940E2eb28930eFb4CeF49B2d1F2C9C1199",
      "value": "0xde0b6b3a7640000"
    }
  ]
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code title="estimateGas.js" %}
```javascript
const axios = require('axios');

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
  jsonrpc: '2.0',
  id: 'getblock.io',
  method: 'eth_estimateGas',
  params: [
    {
      from: '0x742d35Cc6634C0532925a3b844Bc9e7595f8bB45',
      to: '0x8626f6940E2eb28930eFb4CeF49B2d1F2C9C1199',
      value: '0xde0b6b3a7640000'
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
{% code title="estimate_gas.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "eth_estimateGas",
    "params": [
        {
            "from": "0x742d35Cc6634C0532925a3b844Bc9e7595f8bB45",
            "to": "0x8626f6940E2eb28930eFb4CeF49B2d1F2C9C1199",
            "value": "0xde0b6b3a7640000"
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
  "result": "0x5208"
}
```
{% endcode %}

## Response Definition

| Field  | Type   | Description                         |
| ------ | ------ | ----------------------------------- |
| result | string | Estimated gas amount in hexadecimal |

## Use Cases

* Calculate transaction costs before sending
* Set appropriate gas limits
* Budget gas for complex contract interactions
* Optimize transaction parameters
* Prevent out-of-gas errors

## Error Handling

| Error Code | Description                                    |
| ---------- | ---------------------------------------------- |
| -32602     | Invalid params - malformed transaction object  |
| -32603     | Internal error - execution reverted            |
| -32000     | Execution error - insufficient funds or revert |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-estimate.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const gasEstimate = await provider.estimateGas({
  from: '0x742d35Cc6634C0532925a3b844Bc9e7595f8bB45',
  to: '0x8626f6940E2eb28930eFb4CeF49B2d1F2C9C1199',
  value: ethers.parseEther('1.0')
});
console.log(gasEstimate.toString());
```
{% endcode %}
{% endtab %}

{% tab title="ContractKit" %}
{% code title="contractkit-estimate.js" %}
```javascript
const { newKit } = require('@celo/contractkit');

const kit = newKit('https://go.getblock.io/<ACCESS-TOKEN>/');

const gasEstimate = await kit.web3.eth.estimateGas({
  from: '0x742d35Cc6634C0532925a3b844Bc9e7595f8bB45',
  to: '0x8626f6940E2eb28930eFb4CeF49B2d1F2C9C1199',
  value: kit.web3.utils.toWei('1', 'ether')
});
console.log(gasEstimate);
```
{% endcode %}
{% endtab %}
{% endtabs %}
