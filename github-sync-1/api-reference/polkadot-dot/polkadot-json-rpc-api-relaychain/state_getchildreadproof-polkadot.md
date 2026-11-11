---
description: >-
  Example code for the state_getChildReadProof JSON RPC method. Complete guide
  on how to use state_getChildReadProof JSON RPC in GetBlock Web3 documentation.
---

# state\_getChildReadProof - Polkadot

This method returns a Merkle read proof for keys in a child trie at a given block. Child tries are used by pallets that isolate their storage.

## Parameters

| Parameter       | Type   | Required | Description                                                       |
| --------------- | ------ | -------- | ----------------------------------------------------------------- |
| childStorageKey | string | Yes      | The child storage key, hex-encoded                                |
| keys            | array  | Yes      | Array of keys within the child trie, hex-encoded                  |
| at              | string | No       | Block hash to query at. Defaults to the latest block when omitted |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0", "method": "state_getChildReadProof", "params": ["0x3a6368696c645f73746f726167653a64656661756c743a", ["0x26aa394eea5630e07c48ae0c9558cef7b99d880ec681799c0cf30e8886371da9de1e86a9a8c739864cf3cc5ec2bea59fd43593c715fdd31c61141abd04a99fd6822c8558854ccde39a5684e7a56da27d"]], "id": "getblock.io"}'
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
        method: 'state_getChildReadProof',
        params: ["0x3a6368696c645f73746f726167653a64656661756c743a", ["0x26aa394eea5630e07c48ae0c9558cef7b99d880ec681799c0cf30e8886371da9de1e86a9a8c739864cf3cc5ec2bea59fd43593c715fdd31c61141abd04a99fd6822c8558854ccde39a5684e7a56da27d"]],
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
        'method': 'state_getChildReadProof',
        'params': ["0x3a6368696c645f73746f726167653a64656661756c743a", ["0x26aa394eea5630e07c48ae0c9558cef7b99d880ec681799c0cf30e8886371da9de1e86a9a8c739864cf3cc5ec2bea59fd43593c715fdd31c61141abd04a99fd6822c8558854ccde39a5684e7a56da27d"]],
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
        "at": "0x255bc00927df8d33d561792635cbc6bde480a0a505eef5ff28630ece3fc15b32",
        "proof": [
            "0x3701a4...c9"
        ]
    }
}
```

## Response Parameters

| Parameter | Type   | Description                                               |
| --------- | ------ | --------------------------------------------------------- |
| id        | string | Request identifier matching the request                   |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                         |
| result    | object | Object with the block hash and the child-trie proof nodes |

### Result Object

| Field | Type   | Description                     |
| ----- | ------ | ------------------------------- |
| at    | string | Block hash the proof is against |
| proof | array  | Merkle proof nodes, hex-encoded |

## Use Cases

* **Child Trie Proofs**: Verify values stored in a child trie
* **Crowdloan Data**: Prove crowdloan contribution state
* **Light Clients**: Verify child storage without a full node
* **Bridges**: Prove child-trie state cross-chain

## Error Handling

| Error Code | Message          | Description                                            |
| ---------- | ---------------- | ------------------------------------------------------ |
| -32602     | Invalid params   | A parameter is missing or has the wrong type or format |
| -32601     | Method not found | The method is not available on this endpoint           |
| -32603     | Internal error   | The node failed to process the request                 |

## Library Integration

The idiomatic way to call Polkadot RPC is the Polkadot.js API (`@polkadot/api`), which connects over WebSocket and exposes the method as `api.rpc.state.getChildReadProof`.

{% code title="polkadot-js.js" %}
```javascript
import { ApiPromise, WsProvider } from '@polkadot/api';

const provider = new WsProvider('wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');
const api = await ApiPromise.create({ provider });

const result = await api.rpc.state.getChildReadProof('0x3a6368696c645f73746f726167653a64656661756c743a', ['0x26aa394eea5630e07c48ae0c9558cef7b99d880ec681799c0cf30e8886371da9de1e86a9a8c739864cf3cc5ec2bea59fd43593c715fdd31c61141abd04a99fd6822c8558854ccde39a5684e7a56da27d']);
console.log(result.toHuman());
```
{% endcode %}
