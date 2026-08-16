---
description: >-
  Example code for the system_version JSON RPC method. Complete guide on how to
  use system_version JSON RPC in GetBlock Web3 documentation.
---

# system\_version - Polkadot

This method returns the version string of the node's client software.

## Parameters

This method does not accept any parameters.

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0", "method": "system_version", "params": [], "id": "getblock.io"}'
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
        method: 'system_version',
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
        'method': 'system_version',
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
    "result": "1.15.0-a1b2c3d4e5f"
}
```

## Response Parameters

| Parameter | Type   | Description                             |
| --------- | ------ | --------------------------------------- |
| id        | string | Request identifier matching the request |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")       |
| result    | string | Client software version string          |

## Use Cases

* **Node Diagnostics**: Identify the client version
* **Compatibility Checks**: Confirm the node runs a supported release
* **Support Reports**: Include the client version in a bug report
* **Fleet Monitoring**: Track client versions across nodes

## Error Handling

| Error Code | Message          | Description                                            |
| ---------- | ---------------- | ------------------------------------------------------ |
| -32602     | Invalid params   | A parameter is missing or has the wrong type or format |
| -32601     | Method not found | The method is not available on this endpoint           |
| -32603     | Internal error   | The node failed to process the request                 |

## Library Integration

The idiomatic way to call Polkadot RPC is the Polkadot.js API (`@polkadot/api`), which connects over WebSocket and exposes the method as `api.rpc.system.version`.

{% code title="polkadot-js.js" %}
```javascript
import { ApiPromise, WsProvider } from '@polkadot/api';

const provider = new WsProvider('wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');
const api = await ApiPromise.create({ provider });

const result = await api.rpc.system.version();
console.log(result.toHuman());
```
{% endcode %}
