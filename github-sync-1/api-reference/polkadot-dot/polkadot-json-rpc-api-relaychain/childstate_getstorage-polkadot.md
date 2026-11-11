---
description: >-
  Example code for the childstate_getStorage JSON RPC method. Complete guide on
  how to use childstate_getStorage JSON RPC in GetBlock Web3 documentation.
---

# childstate\_getStorage - Polkadot

This method returns the value stored at a key within a child trie, at a given block.

## Parameters

| Parameter       | Type   | Required | Description                                                       |
| --------------- | ------ | -------- | ----------------------------------------------------------------- |
| childStorageKey | string | Yes      | The child storage key, hex-encoded                                |
| key             | string | Yes      | The key within the child trie, hex-encoded                        |
| at              | string | No       | Block hash to query at. Defaults to the latest block when omitted |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0", "method": "childstate_getStorage", "params": ["0x3a6368696c645f73746f726167653a64656661756c743a", "0x26aa394eea5630e07c48ae0c9558cef7b99d880ec681799c0cf30e8886371da9de1e86a9a8c739864cf3cc5ec2bea59fd43593c715fdd31c61141abd04a99fd6822c8558854ccde39a5684e7a56da27d"], "id": "getblock.io"}'
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
        method: 'childstate_getStorage',
        params: ["0x3a6368696c645f73746f726167653a64656661756c743a", "0x26aa394eea5630e07c48ae0c9558cef7b99d880ec681799c0cf30e8886371da9de1e86a9a8c739864cf3cc5ec2bea59fd43593c715fdd31c61141abd04a99fd6822c8558854ccde39a5684e7a56da27d"],
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
        'method': 'childstate_getStorage',
        'params': ["0x3a6368696c645f73746f726167653a64656661756c743a", "0x26aa394eea5630e07c48ae0c9558cef7b99d880ec681799c0cf30e8886371da9de1e86a9a8c739864cf3cc5ec2bea59fd43593c715fdd31c61141abd04a99fd6822c8558854ccde39a5684e7a56da27d"],
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
    "result": "0x00000000000000000100000000000000004a7ba3d15b1d0f00000000000000000000..."
}
```

## Response Parameters

| Parameter | Type   | Description                                                |
| --------- | ------ | ---------------------------------------------------------- |
| id        | string | Request identifier matching the request                    |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                          |
| result    | string | The SCALE-encoded value at the child key, or null if empty |

## Use Cases

* **Child State Reads**: Read a value from a child trie
* **Crowdloan Balances**: Read a contribution amount
* **Pallet Isolation**: Access isolated pallet state
* **Custom Indexing**: Ingest child-trie values

## Error Handling

| Error Code | Message          | Description                                            |
| ---------- | ---------------- | ------------------------------------------------------ |
| -32602     | Invalid params   | A parameter is missing or has the wrong type or format |
| -32601     | Method not found | The method is not available on this endpoint           |
| -32603     | Internal error   | The node failed to process the request                 |

## Library Integration

The idiomatic way to call Polkadot RPC is the Polkadot.js API (`@polkadot/api`), which connects over WebSocket and exposes the method as `api.rpc.childstate.getStorage`.

{% code title="polkadot-js.js" %}
```javascript
import { ApiPromise, WsProvider } from '@polkadot/api';

const provider = new WsProvider('wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');
const api = await ApiPromise.create({ provider });

const result = await api.rpc.childstate.getStorage('0x3a6368696c645f73746f726167653a64656661756c743a', '0x26aa394eea5630e07c48ae0c9558cef7b99d880ec681799c0cf30e8886371da9de1e86a9a8c739864cf3cc5ec2bea59fd43593c715fdd31c61141abd04a99fd6822c8558854ccde39a5684e7a56da27d');
console.log(result.toHuman());
```
{% endcode %}
