---
description: >-
  Example code for the grandpa_proveFinality JSON RPC method. Complete guide on
  how to use grandpa_proveFinality JSON RPC in GetBlock Web3 documentation.
---

# grandpa\_proveFinality - Polkadot

This method returns a GRANDPA finality proof for a given block number, if available. The proof can be used by light clients to verify finality.

## Parameters

| Parameter   | Type    | Required | Description                        |
| ----------- | ------- | -------- | ---------------------------------- |
| blockNumber | integer | Yes      | Block number to prove finality for |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0", "method": "grandpa_proveFinality", "params": [6754000], "id": "getblock.io"}'
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
        method: 'grandpa_proveFinality',
        params: [6754000],
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
        'method': 'grandpa_proveFinality',
        'params': [6754000],
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
    "result": "0x0a01...c3"
}
```

## Response Parameters

| Parameter | Type   | Description                                      |
| --------- | ------ | ------------------------------------------------ |
| id        | string | Request identifier matching the request          |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                |
| result    | string | Encoded finality proof, or null if not available |

## Use Cases

* **Light Clients**: Verify block finality without a full node
* **Bridges**: Prove finalized state to another chain
* **Trust-Minimized Sync**: Follow the chain via finality proofs
* **Audits**: Produce verifiable finality evidence

## Error Handling

| Error Code | Message          | Description                                            |
| ---------- | ---------------- | ------------------------------------------------------ |
| -32602     | Invalid params   | A parameter is missing or has the wrong type or format |
| -32601     | Method not found | The method is not available on this endpoint           |
| -32603     | Internal error   | The node failed to process the request                 |

## Library Integration

The idiomatic way to call Polkadot RPC is the Polkadot.js API (`@polkadot/api`), which connects over WebSocket and exposes the method as `api.rpc.grandpa.proveFinality`.

{% code title="polkadot-js.js" %}
```javascript
import { ApiPromise, WsProvider } from '@polkadot/api';

const provider = new WsProvider('wss://go.getblock.io/<ACCESS-TOKEN>/');
const api = await ApiPromise.create({ provider });

const result = await api.rpc.grandpa.proveFinality(6754000);
console.log(result.toHuman());
```
{% endcode %}
