---
description: >-
  Example code for the state_getPairs JSON RPC method. Complete guide on how to
  use state_getPairs JSON RPC in GetBlock Web3 documentation.
---

# state\_getPairs - Polkadot

This method returns the key/value pairs under a storage prefix, at a given block. It can be expensive on large prefixes and may be restricted on public endpoints.

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
--data-raw '{"jsonrpc": "2.0", "method": "state_getPairs", "params": ["0x26aa394eea5630e07c48ae0c9558cef7"], "id": "getblock.io"}'
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
        method: 'state_getPairs',
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
        'method': 'state_getPairs',
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
        [
            "0x26aa394eea5630e07c48ae0c9558cef7b99d880ec681799c0cf30e8886371da9de1e86a9a8c739864cf3cc5ec2bea59fd43593c715fdd31c61141abd04a99fd6822c8558854ccde39a5684e7a56da27d",
            "0x00000000000000000100000000000000004a7ba3d15b1d0f00000000000000000000..."
        ]
    ]
}
```

## Response Parameters

| Parameter | Type   | Description                                   |
| --------- | ------ | --------------------------------------------- |
| id        | string | Request identifier matching the request       |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")             |
| result    | array  | Array of \[key, value] pairs under the prefix |

## Use Cases

* **Bulk State Export**: Read all entries under a prefix at once
* **Map Snapshots**: Snapshot a full storage map with values
* **Migration Tooling**: Export state for analysis or migration
* **Indexing**: Ingest a whole pallet's storage

## Error Handling

| Error Code | Message          | Description                                                 |
| ---------- | ---------------- | ----------------------------------------------------------- |
| -32602     | Invalid params   | A parameter is missing or has the wrong type or format      |
| -32601     | Method not found | The method is not exposed on this endpoint                  |
| -32000     | Unsafe call      | The method may be restricted on public endpoints for safety |

## Library Integration

The idiomatic way to call Polkadot RPC is the Polkadot.js API (`@polkadot/api`), which connects over WebSocket and exposes the method as `api.rpc.state.getPairs`.

{% code title="polkadot-js.js" %}
```javascript
import { ApiPromise, WsProvider } from '@polkadot/api';

const provider = new WsProvider('wss://go.getblock.io/<ACCESS-TOKEN>/');
const api = await ApiPromise.create({ provider });

const result = await api.rpc.state.getPairs('0x26aa394eea5630e07c48ae0c9558cef7');
console.log(result.toHuman());
```
{% endcode %}
