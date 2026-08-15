---
description: >-
  Example code for the state_getKeysPaged JSON RPC method. Complete guide on how
  to use state_getKeysPaged JSON RPC in GetBlock Web3 documentation.
---

# state\_getKeysPaged - Polkadot

This method returns a page of storage keys starting from a prefix, bounded by a count and an optional start key. It is the safe way to iterate large storage maps.

## Parameters

| Parameter | Type    | Required | Description                                                       |
| --------- | ------- | -------- | ----------------------------------------------------------------- |
| prefix    | string  | Yes      | Storage key prefix, hex-encoded                                   |
| count     | integer | Yes      | Maximum number of keys to return                                  |
| startKey  | string  | No       | Return keys after this key, for pagination                        |
| at        | string  | No       | Block hash to query at. Defaults to the latest block when omitted |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0", "method": "state_getKeysPaged", "params": ["0x26aa394eea5630e07c48ae0c9558cef7", 100], "id": "getblock.io"}'
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
        method: 'state_getKeysPaged',
        params: ["0x26aa394eea5630e07c48ae0c9558cef7", 100],
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
        'method': 'state_getKeysPaged',
        'params': ["0x26aa394eea5630e07c48ae0c9558cef7", 100],
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

| Parameter | Type   | Description                             |
| --------- | ------ | --------------------------------------- |
| id        | string | Request identifier matching the request |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")       |
| result    | array  | Array of up to count storage keys       |

## Use Cases

* **Paged Iteration**: Iterate a large storage map in batches
* **Map Enumeration**: Walk all entries of a map by page
* **Indexing Pipelines**: Stream keys without overloading the node
* **Snapshotting**: Export a storage map incrementally

## Error Handling

| Error Code | Message          | Description                                            |
| ---------- | ---------------- | ------------------------------------------------------ |
| -32602     | Invalid params   | A parameter is missing or has the wrong type or format |
| -32601     | Method not found | The method is not available on this endpoint           |
| -32603     | Internal error   | The node failed to process the request                 |

## Library Integration

The idiomatic way to call Polkadot RPC is the Polkadot.js API (`@polkadot/api`), which connects over WebSocket and exposes the method as `api.rpc.state.getKeysPaged`.

{% code title="polkadot-js.js" %}
```javascript
import { ApiPromise, WsProvider } from '@polkadot/api';

const provider = new WsProvider('wss://go.getblock.io/<ACCESS-TOKEN>/');
const api = await ApiPromise.create({ provider });

const result = await api.rpc.state.getKeysPaged('0x26aa394eea5630e07c48ae0c9558cef7', 100);
console.log(result.toHuman());
```
{% endcode %}
