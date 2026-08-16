---
description: >-
  Example code for the system_chainType JSON RPC method. Complete guide on how
  to use system_chainType JSON RPC in GetBlock Web3 documentation.
---

# system\_chainType - Polkadot

This method returns the type of the chain the node is connected to, such as Live, Local, or Development.

## Parameters

This method does not accept any parameters.

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0", "method": "system_chainType", "params": [], "id": "getblock.io"}'
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
        method: 'system_chainType',
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
        'method': 'system_chainType',
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
    "result": "Live"
}
```

## Response Parameters

| Parameter | Type   | Description                                          |
| --------- | ------ | ---------------------------------------------------- |
| id        | string | Request identifier matching the request              |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                    |
| result    | string | The chain type (Live, Local, Development, or Custom) |

## Use Cases

* **Environment Detection**: Distinguish a live chain from a dev chain
* **Safety Gates**: Block mainnet-only actions on non-live chains
* **Tooling Configuration**: Adapt behavior to the chain type
* **Diagnostics**: Log the chain type for support

## Error Handling

| Error Code | Message          | Description                                            |
| ---------- | ---------------- | ------------------------------------------------------ |
| -32602     | Invalid params   | A parameter is missing or has the wrong type or format |
| -32601     | Method not found | The method is not available on this endpoint           |
| -32603     | Internal error   | The node failed to process the request                 |

## Library Integration

The idiomatic way to call Polkadot RPC is the Polkadot.js API (`@polkadot/api`), which connects over WebSocket and exposes the method as `api.rpc.system.chainType`.

{% code title="polkadot-js.js" %}
```javascript
import { ApiPromise, WsProvider } from '@polkadot/api';

const provider = new WsProvider('wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');
const api = await ApiPromise.create({ provider });

const result = await api.rpc.system.chainType();
console.log(result.toHuman());
```
{% endcode %}
