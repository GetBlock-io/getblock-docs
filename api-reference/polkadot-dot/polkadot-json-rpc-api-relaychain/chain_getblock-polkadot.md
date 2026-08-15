---
description: >-
  Example code for the chain_getBlock JSON RPC method. Complete guide on how to
  use chain_getBlock JSON RPC in GetBlock Web3 documentation.
---

# chain\_getBlock - Polkadot

This method returns a full block by its hash, including the header and the SCALE-encoded extrinsics it contains. If no hash is supplied, the latest block is returned.

## Parameters

| Parameter | Type   | Required | Description                                           |
| --------- | ------ | -------- | ----------------------------------------------------- |
| hash      | string | No       | Block hash. Defaults to the latest block when omitted |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0", "method": "chain_getBlock", "params": ["0x255bc00927df8d33d561792635cbc6bde480a0a505eef5ff28630ece3fc15b32"], "id": "getblock.io"}'
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
        method: 'chain_getBlock',
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
    'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'chain_getBlock',
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
        "block": {
            "extrinsics": [
                "0x280403000b406017cb7b01"
            ],
            "header": {
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
        },
        "justifications": null
    }
}
```

## Response Parameters

| Parameter | Type   | Description                                        |
| --------- | ------ | -------------------------------------------------- |
| id        | string | Request identifier matching the request            |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                  |
| result    | object | Signed block object with its header and extrinsics |

### block.header

| Field          | Type   | Description                                       |
| -------------- | ------ | ------------------------------------------------- |
| parentHash     | string | Hash of the parent block header                   |
| number         | string | Block number, hex-encoded                         |
| stateRoot      | string | Root hash of the state trie after the block       |
| extrinsicsRoot | string | Root hash of the extrinsics trie                  |
| digest         | object | Consensus digest logs (BABE, GRANDPA, and others) |

### block

| Field      | Type   | Description                           |
| ---------- | ------ | ------------------------------------- |
| extrinsics | array  | SCALE-encoded extrinsics in the block |
| header     | object | The block header                      |

## Use Cases

* **Block Explorers**: Render a block's header and extrinsics
* **Chain Indexing**: Ingest blocks and decode their extrinsics
* **Transaction Tracing**: Locate an extrinsic within a block
* **Auditing**: Read the raw contents of a specific block

## Error Handling

| Error Code | Message          | Description                                            |
| ---------- | ---------------- | ------------------------------------------------------ |
| -32602     | Invalid params   | A parameter is missing or has the wrong type or format |
| -32601     | Method not found | The method is not available on this endpoint           |
| -32603     | Internal error   | The node failed to process the request                 |

## Library Integration

The idiomatic way to call Polkadot RPC is the Polkadot.js API (`@polkadot/api`), which connects over WebSocket and exposes the method as `api.rpc.chain.getBlock`.

{% code title="polkadot-js.js" %}
```javascript
import { ApiPromise, WsProvider } from '@polkadot/api';

const provider = new WsProvider('wss://go.getblock.io/<ACCESS-TOKEN>/');
const api = await ApiPromise.create({ provider });

const result = await api.rpc.chain.getBlock('0x255bc00927df8d33d561792635cbc6bde480a0a505eef5ff28630ece3fc15b32');
console.log(result.toHuman());
```
{% endcode %}
