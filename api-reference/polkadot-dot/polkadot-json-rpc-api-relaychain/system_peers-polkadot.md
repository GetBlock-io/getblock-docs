---
description: >-
  Example code for the system_peers JSON RPC method. Complete guide on how to
  use system_peers JSON RPC in GetBlock Web3 documentation.
---

# system\_peers - Polkadot

This method returns details of the peers currently connected to the node. It may be restricted on public endpoints.

## Parameters

This method does not accept any parameters.

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0", "method": "system_peers", "params": [], "id": "getblock.io"}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
        jsonrpc: '2.0',
        method: 'system_peers',
        params: [],
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'system_peers',
        'params': [],
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
        {
            "peerId": "12D3KooWSueCPH3puP2PcvqPJdNaDNF3jMZjtJtDiSy35pWrbt5h",
            "roles": "FULL",
            "protocolVersion": 1,
            "bestHash": "0x255bc00927df8d33d561792635cbc6bde480a0a505eef5ff28630ece3fc15b32",
            "bestNumber": 6754362
        }
    ]
}
```

## Response Parameters

| Parameter | Type   | Description                             |
| --------- | ------ | --------------------------------------- |
| id        | string | Request identifier matching the request |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")       |
| result    | array  | Array of connected peer objects         |

## Use Cases

* **Connectivity Diagnostics**: Inspect a node's peer connections
* **Network Analysis**: Study peer roles and best blocks
* **Troubleshooting**: Investigate sync or peering issues
* **Monitoring**: Track peer quality

## Error Handling

| Error Code | Message          | Description                                                 |
| ---------- | ---------------- | ----------------------------------------------------------- |
| -32602     | Invalid params   | A parameter is missing or has the wrong type or format      |
| -32601     | Method not found | The method is not exposed on this endpoint                  |
| -32000     | Unsafe call      | The method may be restricted on public endpoints for safety |

## Library Integration

The idiomatic way to call Polkadot RPC is the Polkadot.js API (`@polkadot/api`), which connects over WebSocket and exposes the method as `api.rpc.system.peers`.

{% code title="polkadot-js.js" %}
```javascript
import { ApiPromise, WsProvider } from '@polkadot/api';

const provider = new WsProvider('wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');
const api = await ApiPromise.create({ provider });

const result = await api.rpc.system.peers();
console.log(result.toHuman());
```
{% endcode %}
