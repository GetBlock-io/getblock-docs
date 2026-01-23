---
description: >-
  Example code for the eth_getStorageAt JSON-RPC method. Complete guide on how
  to use eth_getStorageAt JSON-RPC in GetBlock Web3 documentation.
---

# eth\_getStorageAt - Celo

This method returns the value from a storage position at a given address on the Celo network. This is useful for reading raw contract storage, including private variables that aren't exposed through public functions.

## Parameters

| Parameter   | Type   | Required | Description                                             |
| ----------- | ------ | -------- | ------------------------------------------------------- |
| address     | string | Yes      | Contract address                                        |
| position    | string | Yes      | Storage position as hex                                 |
| blockNumber | string | Yes      | Block number in hex, or "latest", "earliest", "pending" |

## Request Example

{% tabs %}
{% tab title="First Tab" %}
{% code title="cURL" %}
```bash
curl -X POST https://go.getblock.io/<ACCESS-TOKEN>/ \
-H "Content-Type: application/json" \
-d '{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "eth_getStorageAt",
  "params": [
    "0x765DE816845861e75A25fCA122bb6898B8B1282a",
    "0x0",
    "latest"
  ]
}'
```
{% endcode %}
{% endtab %}

{% tab title="Second Tab" %}
{% code title="JavaScript (Axios)" %}
```javascript
const axios = require('axios');

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
  jsonrpc: '2.0',
  id: 'getblock.io',
  method: 'eth_getStorageAt',
  params: [
    '0x765DE816845861e75A25fCA122bb6898B8B1282a',
    '0x0',
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

{% tab title="Untitled" %}
{% code title="Python" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "eth_getStorageAt",
    "params": [
        "0x765DE816845861e75A25fCA122bb6898B8B1282a",
        "0x0",
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

{% code title="Response JSON" %}
```json
{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": "0x0000000000000000000000000000000000000000000000000000000000000001"
}
```
{% endcode %}

## Response Definition

| Field  | Type   | Description                         |
| ------ | ------ | ----------------------------------- |
| result | string | Storage value as 32-byte hex string |

## Use Cases

* Read contract storage directly
* Access private variables
* Analyze proxy contract state
* Debug contract behavior

## Error Handling

| Error Code | Description                                    |
| ---------- | ---------------------------------------------- |
| -32602     | Invalid params - malformed address or position |
| -32603     | Internal error - node processing issues        |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="Ethers.js" overflow="wrap" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const storage = await provider.getStorage('0x765DE816845861e75A25fCA122bb6898B8B1282a', 0);
console.log(storage);
```
{% endcode %}
{% endtab %}

{% tab title="ContractKit" %}
{% code title="ContractKit" overflow="wrap" %}
```javascript
const { newKit } = require('@celo/contractkit');

const kit = newKit('https://go.getblock.io/<ACCESS-TOKEN>/');

const storage = await kit.web3.eth.getStorageAt('0x765DE816845861e75A25fCA122bb6898B8B1282a', 0);
console.log(storage);
```
{% endcode %}
{% endtab %}
{% endtabs %}
