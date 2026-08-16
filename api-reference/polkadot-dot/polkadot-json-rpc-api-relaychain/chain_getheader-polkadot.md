---
description: >-
  Example code for the chain_getHeader JSON RPC method. Complete guide on how to
  use chain_getHeader JSON RPC in GetBlock Web3 documentation.
---

# chain\_getHeader - Polkadot

This method returns the header of a block by its hash, without the extrinsics. If no hash is supplied, the latest block header is returned.

## Parameters

| Parameter | Type   | Required | Description                                           |
| --------- | ------ | -------- | ----------------------------------------------------- |
| hash      | string | No       | Block hash. Defaults to the latest block when omitted |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0", "method": "chain_getHeader", "params": ["0x255bc00927df8d33d561792635cbc6bde480a0a505eef5ff28630ece3fc15b32"], "id": "getblock.io"}'
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
        method: 'chain_getHeader',
        params: ["0x255bc00927df8d33d561792635cbc6bde480a0a505eef5ff28630ece3fc15b32"],
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
        'method': 'chain_getHeader',
        'params': ["0x255bc00927df8d33d561792635cbc6bde480a0a505eef5ff28630ece3fc15b32"],
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
        "digest": {
            "logs": [
                "0x0642414245b5010392...",
                "0x0542414245010114a5..."
            ]
        },
        "extrinsicsRoot": "0xada254ef8321e28a6667ad75659be6464944174bd5540667da94590a0a4a596f",
        "number": "0x67103a",
        "parentHash": "0x570e3f417a41646d9b978bf2ac3d68be48bb0f73082825f438af58a37cfe0ef8",
        "stateRoot": "0x176c67eca385c24403ba774550ff75dcfa652d6d6cf2d2ecbccf56e513db601c"
    }
}
```

## Response Parameters

| Parameter | Type   | Description                             |
| --------- | ------ | --------------------------------------- |
| id        | string | Request identifier matching the request |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")       |
| result    | object | Block header object                     |

### Result Object

| Field          | Type   | Description                                       |
| -------------- | ------ | ------------------------------------------------- |
| parentHash     | string | Hash of the parent block header                   |
| number         | string | Block number, hex-encoded                         |
| stateRoot      | string | Root hash of the state trie after the block       |
| extrinsicsRoot | string | Root hash of the extrinsics trie                  |
| digest         | object | Consensus digest logs (BABE, GRANDPA, and others) |

## Use Cases

* **Lightweight Sync**: Read headers without downloading full blocks
* **State Roots**: Read the state root for proof verification
* **Chain Following**: Track the head by reading successive headers
* **Digest Inspection**: Read consensus logs from the header digest

## Error Handling

| Error Code | Message          | Description                                            |
| ---------- | ---------------- | ------------------------------------------------------ |
| -32602     | Invalid params   | A parameter is missing or has the wrong type or format |
| -32601     | Method not found | The method is not available on this endpoint           |
| -32603     | Internal error   | The node failed to process the request                 |

## Library Integration

The idiomatic way to call Polkadot RPC is the Polkadot.js API (`@polkadot/api`), which connects over WebSocket and exposes the method as `api.rpc.chain.getHeader`.

{% code title="polkadot-js.js" %}
```javascript
import { ApiPromise, WsProvider } from '@polkadot/api';

const provider = new WsProvider('wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');
const api = await ApiPromise.create({ provider });

const result = await api.rpc.chain.getHeader('0x255bc00927df8d33d561792635cbc6bde480a0a505eef5ff28630ece3fc15b32');
console.log(result.toHuman());
```
{% endcode %}
