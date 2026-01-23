---
description: >-
  Example code for the net_version JSON-RPC method. Complete guide on how to use
  net_version JSON-RPC in GetBlock Web3 documentation.
---

# net\_version - Celo

This method returns the current network ID of the Celo node.

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
  "method": "net_version",
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
  method: 'net_version',
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
    "method": "net_version",
    "params": []
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
  "result": "42220"
}
```

## Response Definition

| Field  | Type   | Description                    |
| ------ | ------ | ------------------------------ |
| result | string | Network ID as a decimal string |

## Use Cases

* Verify connection to correct network
* Distinguish mainnet from testnet
* Network detection in multi-chain apps

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

const network = await provider.getNetwork();
console.log('Network ID:', network.chainId.toString());
```
{% endtab %}

{% tab title="ContractKit" %}
```javascript
const { newKit } = require('@celo/contractkit');

const kit = newKit('https://go.getblock.io/<ACCESS-TOKEN>/');

const networkId = await kit.web3.eth.net.getId();
console.log('Network ID:', networkId);
```
{% endtab %}
{% endtabs %}
