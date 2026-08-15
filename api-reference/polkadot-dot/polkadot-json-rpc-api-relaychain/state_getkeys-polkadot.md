---
description: >-
  Example code for the state_getKeys JSON RPC method. Complete guide on how to
  use state_getKeys JSON RPC in GetBlock Web3 documentation.
---

# state\_getKeys - Polkadot

This method returns the storage keys that start with a given prefix, at a given block. On large tries this can be expensive; state\_getKeysPaged is preferred for pagination.

## Parameters

| Parameter | Type   | Required | Description                                                       |
| --------- | ------ | -------- | ----------------------------------------------------------------- |
| prefix    | string | Yes      | Storage key prefix, hex-encoded                                   |
| at        | string | No       | Block hash to query at. Defaults to the latest block when omitted |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0", "method": "state_getKeys", "params": ["0x26aa394eea5630e07c48ae0c9558cef7"], "id": "getblock.io"}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch('https://go.getblock.io/<ACCESS-TOKEN>/', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
        jsonrpc: '2.0',
        method: 'state_getKeys',
        params: ["0x26aa394eea5630e07c48ae0c9558cef7"],
        id: 'getblock.io'
    })
});

const data = await response.json();
console.log(data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

response = requests.post(
    'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'state_getKeys',
        'params': ["0x26aa394eea5630e07c48ae0c9558cef7"],
        'id': 'getblock.io'
    }
)

print(response.json()['result'])
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": [
        "0x26aa394eea5630e07c48ae0c9558cef7b99d880ec681799c0cf30e8886371da9de1e86a9a8c739864cf3cc5ec2bea59fd43593c715fdd31c61141abd04a99fd6822c8558854ccde39a5684e7a56da27d"
    ]
}
```

## Response Parameters

| Parameter | Type   | Description                               |
| --------- | ------ | ----------------------------------------- |
| id        | string | Request identifier matching the request   |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")         |
| result    | array  | Array of storage keys matching the prefix |

## Use Cases

* **Storage Enumeration**: List all keys under a pallet or map
* **Map Iteration**: Enumerate the entries of a storage map
* **Indexing**: Discover keys to read in bulk
* **Auditing**: Inspect the key space of a storage item

## Error Handling

| Error Code | Message          | Description                                            |
| ---------- | ---------------- | ------------------------------------------------------ |
| -32602     | Invalid params   | A parameter is missing or has the wrong type or format |
| -32601     | Method not found | The method is not available on this endpoint           |
| -32603     | Internal error   | The node failed to process the request                 |

## Library Integration

The idiomatic way to call Polkadot RPC is the Polkadot.js API (`@polkadot/api`), which connects over WebSocket and exposes the method as `api.rpc.state.getKeys`.

{% code title="polkadot-js.js" %}
```javascript
import { ApiPromise, WsProvider } from '@polkadot/api';

const provider = new WsProvider('wss://go.getblock.io/<ACCESS-TOKEN>/');
const api = await ApiPromise.create({ provider });

const result = await api.rpc.state.getKeys('0x26aa394eea5630e07c48ae0c9558cef7');
console.log(result.toHuman());
```
{% endcode %}
