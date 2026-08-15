---
description: >-
  Example code for the state_call JSON RPC method. Complete guide on how to use
  state_call JSON RPC in GetBlock Web3 documentation.
---

# state\_call - Polkadot

This method executes a runtime API call by name with SCALE-encoded arguments and returns the SCALE-encoded result, at a given block. It exposes runtime APIs not surfaced as dedicated RPC methods.

## Parameters

| Parameter | Type   | Required | Description                                                       |
| --------- | ------ | -------- | ----------------------------------------------------------------- |
| name      | string | Yes      | Runtime API method name, such as Metadata\_metadata               |
| bytes     | string | Yes      | SCALE-encoded call arguments, hex-encoded (0x for none)           |
| at        | string | No       | Block hash to query at. Defaults to the latest block when omitted |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0", "method": "state_call", "params": ["AccountNonceApi_account_nonce", "0x0000000000000000000000000000000000000000000000000000000000000000"], "id": "getblock.io"}'
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
        method: 'state_call',
        params: ["AccountNonceApi_account_nonce", "0x0000000000000000000000000000000000000000000000000000000000000000"],
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
        'method': 'state_call',
        'params': ["AccountNonceApi_account_nonce", "0x0000000000000000000000000000000000000000000000000000000000000000"],
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
    "result": "0xd2040000"
}
```

## Response Parameters

| Parameter | Type   | Description                                           |
| --------- | ------ | ----------------------------------------------------- |
| id        | string | Request identifier matching the request               |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                     |
| result    | string | SCALE-encoded result of the runtime call, hex-encoded |

## Use Cases

* **Runtime APIs**: Call runtime APIs like fee or nonce queries
* **Advanced Reads**: Access logic not exposed as a named RPC
* **Fee Computation**: Invoke TransactionPaymentApi methods
* **Custom Runtimes**: Call parachain-specific runtime APIs

## Error Handling

| Error Code | Message          | Description                                            |
| ---------- | ---------------- | ------------------------------------------------------ |
| -32602     | Invalid params   | A parameter is missing or has the wrong type or format |
| -32601     | Method not found | The method is not available on this endpoint           |
| -32603     | Internal error   | The node failed to process the request                 |

## Library Integration

The idiomatic way to call Polkadot RPC is the Polkadot.js API (`@polkadot/api`), which connects over WebSocket and exposes the method as `api.rpc.state.call`.

{% code title="polkadot-js.js" %}
```javascript
import { ApiPromise, WsProvider } from '@polkadot/api';

const provider = new WsProvider('wss://go.getblock.io/<ACCESS-TOKEN>/');
const api = await ApiPromise.create({ provider });

const result = await api.rpc.state.call('AccountNonceApi_account_nonce', '0x...');
console.log(result.toHuman());
```
{% endcode %}
