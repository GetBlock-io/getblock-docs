# dot\_mmr\_verifyproof

This method verifies an MMR proof for a set of leaves against the node's MMR state and returns whether the proof is valid.

## Parameters

| Parameter | Type   | Required | Description                           |
| --------- | ------ | -------- | ------------------------------------- |
| leaves    | string | Yes      | SCALE-encoded MMR leaves, hex-encoded |
| proof     | string | Yes      | SCALE-encoded MMR proof, hex-encoded  |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0", "method": "mmr_verifyProof", "params": ["0xc501...", "0x0401..."], "id": "getblock.io"}'
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
        method: 'mmr_verifyProof',
        params: ["0xc501...", "0x0401..."],
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
        'method': 'mmr_verifyProof',
        'params': ["0xc501...", "0x0401..."],
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
    "result": true
}
```

## Response Parameters

| Parameter | Type    | Description                             |
| --------- | ------- | --------------------------------------- |
| id        | string  | Request identifier matching the request |
| jsonrpc   | string  | JSON-RPC protocol version ("2.0")       |
| result    | boolean | true if the proof is valid              |

## Use Cases

* **Proof Verification**: Validate an MMR proof node-side
* **Bridge Relayers**: Confirm proofs before relaying
* **Testing**: Verify generated proofs in development
* **Auditing**: Independently check MMR proofs

## Error Handling

| Error Code | Message          | Description                                            |
| ---------- | ---------------- | ------------------------------------------------------ |
| -32602     | Invalid params   | A parameter is missing or has the wrong type or format |
| -32601     | Method not found | The method is not available on this endpoint           |
| -32603     | Internal error   | The node failed to process the request                 |

## Library Integration

The idiomatic way to call Polkadot RPC is the Polkadot.js API (`@polkadot/api`), which connects over WebSocket and exposes the method as `api.rpc.mmr.verifyProof`.

{% code title="polkadot-js.js" %}
```javascript
import { ApiPromise, WsProvider } from '@polkadot/api';

const provider = new WsProvider('wss://go.getblock.io/<ACCESS-TOKEN>/');
const api = await ApiPromise.create({ provider });

const result = await api.rpc.mmr.verifyProof('0xc501...', '0x0401...');
console.log(result.toHuman());
```
{% endcode %}
