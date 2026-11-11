---
description: >-
  Example code for the state_traceBlock JSON RPC method. Complete guide on how
  to use state_traceBlock JSON RPC in GetBlock Web3 documentation.
---

# state\_traceBlock - Polkadot

This method re-executes a block and returns storage access and event traces filtered by the given targets, storage keys, and methods. It is an advanced diagnostic method and may be restricted on public endpoints.

## Parameters

| Parameter   | Type   | Required | Description                                   |
| ----------- | ------ | -------- | --------------------------------------------- |
| block       | string | Yes      | Block hash to trace                           |
| targets     | string | No       | Comma-separated trace targets, or null        |
| storageKeys | string | No       | Comma-separated storage key prefixes, or null |
| methods     | string | No       | Comma-separated methods to trace, or null     |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0", "method": "state_traceBlock", "params": ["0x255bc00927df8d33d561792635cbc6bde480a0a505eef5ff28630ece3fc15b32", null, null, null], "id": "getblock.io"}'
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
        method: 'state_traceBlock',
        params: ["0x255bc00927df8d33d561792635cbc6bde480a0a505eef5ff28630ece3fc15b32", null, null, null],
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
        'method': 'state_traceBlock',
        'params': ["0x255bc00927df8d33d561792635cbc6bde480a0a505eef5ff28630ece3fc15b32", null, null, null],
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
    "result": {
        "block": "0x255bc00927df8d33d561792635cbc6bde480a0a505eef5ff28630ece3fc15b32",
        "tracingTargets": "state",
        "storageKeys": "",
        "spans": [],
        "events": []
    }
}
```

## Response Parameters

| Parameter | Type   | Description                              |
| --------- | ------ | ---------------------------------------- |
| id        | string | Request identifier matching the request  |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")        |
| result    | object | Block trace object with spans and events |

## Use Cases

* **Deep Diagnostics**: Trace storage access within a block
* **Performance Analysis**: Investigate expensive extrinsics
* **Runtime Debugging**: Inspect execution spans
* **Forensics**: Reconstruct what a block touched

## Error Handling

| Error Code | Message          | Description                                                 |
| ---------- | ---------------- | ----------------------------------------------------------- |
| -32602     | Invalid params   | A parameter is missing or has the wrong type or format      |
| -32601     | Method not found | The method is not exposed on this endpoint                  |
| -32000     | Unsafe call      | The method may be restricted on public endpoints for safety |

## Library Integration

The idiomatic way to call Polkadot RPC is the Polkadot.js API (`@polkadot/api`), which connects over WebSocket and exposes the method as `api.rpc.state.traceBlock`.

{% code title="polkadot-js.js" %}
```javascript
import { ApiPromise, WsProvider } from '@polkadot/api';

const provider = new WsProvider('wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');
const api = await ApiPromise.create({ provider });

const result = await api.rpc.state.traceBlock('0x255bc00927df8d33d561792635cbc6bde480a0a505eef5ff28630ece3fc15b32', null, null, null);
console.log(result.toHuman());
```
{% endcode %}
