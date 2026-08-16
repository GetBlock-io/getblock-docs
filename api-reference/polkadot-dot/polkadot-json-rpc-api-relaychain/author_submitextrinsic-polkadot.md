---
description: >-
  Example code for the author_submitExtrinsic JSON RPC method. Complete guide on
  how to use author_submitExtrinsic JSON RPC in GetBlock Web3 documentation.
---

# author\_submitExtrinsic - Polkadot

This method submits a signed extrinsic to the node's transaction pool for inclusion in a block and returns its hash. The extrinsic must be constructed and signed by the caller.

## Parameters

| Parameter | Type   | Required | Description                                    |
| --------- | ------ | -------- | ---------------------------------------------- |
| extrinsic | string | Yes      | A signed, SCALE-encoded extrinsic, hex-encoded |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0", "method": "author_submitExtrinsic", "params": ["0x4d028400d43593c715fdd31c61141abd04a99fd6822c8558854ccde39a5684e7a56da27d01..."], "id": "getblock.io"}'
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
        method: 'author_submitExtrinsic',
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'author_submitExtrinsic',
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
    "result": "0x8e6c1c623b9d2a2c0e6f6c3b8f0a5d5d1a4b8c9e0f1a2b3c4d5e6f7a8b9c0d1e"
}
```

## Response Parameters

| Parameter | Type   | Description                             |
| --------- | ------ | --------------------------------------- |
| id        | string | Request identifier matching the request |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")       |
| result    | string | The hash of the submitted extrinsic     |

## Use Cases

* **Transaction Submission**: Broadcast a signed transaction to the network
* **Transfers**: Submit a balance transfer extrinsic
* **Staking Operations**: Submit bond, nominate, or payout extrinsics
* **Governance**: Submit votes and proposals

## Error Handling

| Error Code | Message          | Description                                            |
| ---------- | ---------------- | ------------------------------------------------------ |
| -32602     | Invalid params   | A parameter is missing or has the wrong type or format |
| -32601     | Method not found | The method is not available on this endpoint           |
| -32603     | Internal error   | The node failed to process the request                 |

## Library Integration

The idiomatic way to call Polkadot RPC is the Polkadot.js API (`@polkadot/api`), which connects over WebSocket and exposes the method as `api.rpc.author.submitExtrinsic`.

{% code title="polkadot-js.js" %}
```javascript
import { ApiPromise, WsProvider } from '@polkadot/api';

const provider = new WsProvider('wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');
const api = await ApiPromise.create({ provider });

const result = await api.rpc.author.submitExtrinsic('0x4d028400d43593c715fdd31c61141abd04a99fd6822c8558854ccde39a5684e7a56da27d01...');
console.log(result.toHuman());
```
{% endcode %}
