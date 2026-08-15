---
description: >-
  Example code for the system_dryRun JSON RPC method. Complete guide on how to
  use system_dryRun JSON RPC in GetBlock Web3 documentation.
---

# system\_dryRun - Polkadot

This method dry-runs an extrinsic against the current state and returns the encoded result without submitting it. It is used to check whether a transaction would succeed. It may be restricted on public endpoints.

## Parameters

| Parameter | Type   | Required | Description                                                       |
| --------- | ------ | -------- | ----------------------------------------------------------------- |
| extrinsic | string | Yes      | A signed, SCALE-encoded extrinsic, hex-encoded                    |
| at        | string | No       | Block hash to query at. Defaults to the latest block when omitted |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0", "method": "system_dryRun", "params": ["0x4d028400d43593c715fdd31c61141abd04a99fd6822c8558854ccde39a5684e7a56da27d01..."], "id": "getblock.io"}'
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
        method: 'system_dryRun',
        params: ["0x4d028400d43593c715fdd31c61141abd04a99fd6822c8558854ccde39a5684e7a56da27d01..."],
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
        'method': 'system_dryRun',
        'params': ["0x4d028400d43593c715fdd31c61141abd04a99fd6822c8558854ccde39a5684e7a56da27d01..."],
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
    "result": "0x0100"
}
```

## Response Parameters

| Parameter | Type   | Description                             |
| --------- | ------ | --------------------------------------- |
| id        | string | Request identifier matching the request |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")       |
| result    | string | SCALE-encoded ApplyExtrinsicResult      |

## Use Cases

* **Pre-Flight Checks**: Verify an extrinsic before submitting it
* **Error Prevention**: Detect failures without paying fees
* **Simulation**: Preview the outcome of a transaction
* **Wallet UX**: Warn users of likely failures

## Error Handling

| Error Code | Message          | Description                                                 |
| ---------- | ---------------- | ----------------------------------------------------------- |
| -32602     | Invalid params   | A parameter is missing or has the wrong type or format      |
| -32601     | Method not found | The method is not exposed on this endpoint                  |
| -32000     | Unsafe call      | The method may be restricted on public endpoints for safety |

## Library Integration

The idiomatic way to call Polkadot RPC is the Polkadot.js API (`@polkadot/api`), which connects over WebSocket and exposes the method as `api.rpc.system.dryRun`.

{% code title="polkadot-js.js" %}
```javascript
import { ApiPromise, WsProvider } from '@polkadot/api';

const provider = new WsProvider('wss://go.getblock.io/<ACCESS-TOKEN>/');
const api = await ApiPromise.create({ provider });

const result = await api.rpc.system.dryRun('0x4d028400d43593c715fdd31c61141abd04a99fd6822c8558854ccde39a5684e7a56da27d01...');
console.log(result.toHuman());
```
{% endcode %}
