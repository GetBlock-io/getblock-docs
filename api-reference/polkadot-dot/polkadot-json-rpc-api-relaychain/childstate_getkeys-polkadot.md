---
description: >-
  Example code for the childstate_getKeys JSON RPC method. Complete guide on how
  to use childstate_getKeys JSON RPC in GetBlock Web3 documentation.
---

# childstate\_getKeys - Polkadot

This method returns the keys in a child trie that match a prefix, at a given block. Child tries isolate the storage of certain pallets.

## Parameters

| Parameter       | Type   | Required | Description                                                       |
| --------------- | ------ | -------- | ----------------------------------------------------------------- |
| childStorageKey | string | Yes      | The child storage key, hex-encoded                                |
| prefix          | string | Yes      | Key prefix within the child trie, hex-encoded                     |
| at              | string | No       | Block hash to query at. Defaults to the latest block when omitted |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0", "method": "childstate_getKeys", "params": ["0x3a6368696c645f73746f726167653a64656661756c743a", "0x"], "id": "getblock.io"}'
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
        method: 'childstate_getKeys',
        params: ["0x3a6368696c645f73746f726167653a64656661756c743a", "0x"],
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
        'method': 'childstate_getKeys',
        'params': ["0x3a6368696c645f73746f726167653a64656661756c743a", "0x"],
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

| Parameter | Type   | Description                                         |
| --------- | ------ | --------------------------------------------------- |
| id        | string | Request identifier matching the request             |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                   |
| result    | array  | Array of keys in the child trie matching the prefix |

## Use Cases

* **Child Storage Enumeration**: List keys within a child trie
* **Crowdloan Indexing**: Enumerate crowdloan contribution keys
* **Pallet Isolation**: Inspect isolated pallet storage
* **Auditing**: Explore a child trie's key space

## Error Handling

| Error Code | Message          | Description                                            |
| ---------- | ---------------- | ------------------------------------------------------ |
| -32602     | Invalid params   | A parameter is missing or has the wrong type or format |
| -32601     | Method not found | The method is not available on this endpoint           |
| -32603     | Internal error   | The node failed to process the request                 |

## Library Integration

The idiomatic way to call Polkadot RPC is the Polkadot.js API (`@polkadot/api`), which connects over WebSocket and exposes the method as `api.rpc.childstate.getKeys`.

{% code title="polkadot-js.js" %}
```javascript
import { ApiPromise, WsProvider } from '@polkadot/api';

const provider = new WsProvider('wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');
const api = await ApiPromise.create({ provider });

const result = await api.rpc.childstate.getKeys('0x3a6368696c645f73746f726167653a64656661756c743a', '0x');
console.log(result.toHuman());
```
{% endcode %}
