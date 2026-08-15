# dot\_mmr\_generateproof

This method generates an MMR proof for one or more block numbers, returning the leaves and the proof needed to verify them against the MMR root.

## Parameters

| Parameter            | Type    | Required | Description                                                       |
| -------------------- | ------- | -------- | ----------------------------------------------------------------- |
| blockNumbers         | array   | Yes      | Block numbers to prove                                            |
| bestKnownBlockNumber | integer | No       | Block number whose MMR state to prove against                     |
| at                   | string  | No       | Block hash to query at. Defaults to the latest block when omitted |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0", "method": "mmr_generateProof", "params": [[6754000]], "id": "getblock.io"}'
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
        method: 'mmr_generateProof',
        params: [[6754000]],
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
        'method': 'mmr_generateProof',
        'params': [[6754000]],
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
        "blockHash": "0x255bc00927df8d33d561792635cbc6bde480a0a505eef5ff28630ece3fc15b32",
        "leaves": "0xc501...",
        "proof": "0x0401..."
    }
}
```

## Response Parameters

| Parameter | Type   | Description                             |
| --------- | ------ | --------------------------------------- |
| id        | string | Request identifier matching the request |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")       |
| result    | object | MMR proof object                        |

### Result Object

| Field     | Type   | Description                           |
| --------- | ------ | ------------------------------------- |
| blockHash | string | Block the proof is generated at       |
| leaves    | string | SCALE-encoded MMR leaves, hex-encoded |
| proof     | string | SCALE-encoded MMR proof, hex-encoded  |

## Use Cases

* **Bridge Proofs**: Generate proofs for cross-chain verification
* **Historical Proofs**: Prove a past block belongs to the chain
* **Light Clients**: Provide verifiable history to light clients
* **Interoperability**: Feed BEEFY-based bridge relayers

## Error Handling

| Error Code | Message          | Description                                            |
| ---------- | ---------------- | ------------------------------------------------------ |
| -32602     | Invalid params   | A parameter is missing or has the wrong type or format |
| -32601     | Method not found | The method is not available on this endpoint           |
| -32603     | Internal error   | The node failed to process the request                 |

## Library Integration

The idiomatic way to call Polkadot RPC is the Polkadot.js API (`@polkadot/api`), which connects over WebSocket and exposes the method as `api.rpc.mmr.generateProof`.

{% code title="polkadot-js.js" %}
```javascript
import { ApiPromise, WsProvider } from '@polkadot/api';

const provider = new WsProvider('wss://go.getblock.io/<ACCESS-TOKEN>/');
const api = await ApiPromise.create({ provider });

const result = await api.rpc.mmr.generateProof([6754000]);
console.log(result.toHuman());
```
{% endcode %}
