---
description: >-
  Example code for the web3_clientVersion json-rpc method. Сomplete guide on how
  to use web3_clientVersion json-rpc in GetBlock.io Web3 documentation.
---

# web3\_clientVersion - Optimism

This method returns a string identifying the current client implementation and version. For Optimism, this will typically include information about the op-geth client being used. This is useful for debugging, compatibility checking, and infrastructure monitoring.

### Parameters

* None

### Request Sample

{% tabs %}
{% tab title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "web3_clientVersion",
    "params": [],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="wss" %}
```bash
wscat -c wss://go.getblock.io/<ACCESS-TOKEN>/

# wait for connection and send the request body 
{"jsonrpc": "2.0", "method": "web3_clientVersion", "params": [], "id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

### Response

A successful response returns the following:

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "Geth/v1.101702.0-rc.1-3de88058-20260407/linux-amd64/go1.24.13"
}
```

### Response Parameters

* **`id`**: A unique request identifier, matching the `id` sent in the request body.
* **`jsonrpc`**: Specifies the use of JSON-RPC version 2.0.
* **`result`**: A string containing the client name, version, platform, and runtime information.

### Use Case

The web3\_clientVersion method is commonly used for:

* **Client identification**
* **Version compatibility checking**
* **Debugging**
* **Infrastructure documentation**

## Error Handling

| Error Code | Description    |
| ---------- | -------------- |
| -32603     | Internal error |

## Web3  Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const version = await provider.send('web3_clientVersion', []);
console.log('Client Version:', version);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { optimism } from 'viem/chains';

const client = createPublicClient({
    chain: optimism,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const version = await client.request({ method: 'web3_clientVersion' });
console.log('Client Version:', version);
```
{% endcode %}
{% endtab %}
{% endtabs %}
