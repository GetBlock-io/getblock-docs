# dot\_beefy\_getfinalizedhead

This method returns the hash of the latest block finalized by BEEFY. BEEFY is a companion protocol to GRANDPA that produces bridge-friendly finality proofs.

## Parameters

This method does not accept any parameters.

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0", "method": "beefy_getFinalizedHead", "params": [], "id": "getblock.io"}'
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
        method: 'beefy_getFinalizedHead',
        params: [],
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
        'method': 'beefy_getFinalizedHead',
        'params': [],
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
    "result": "0x255bc00927df8d33d561792635cbc6bde480a0a505eef5ff28630ece3fc15b32"
}
```

## Response Parameters

| Parameter | Type   | Description                              |
| --------- | ------ | ---------------------------------------- |
| id        | string | Request identifier matching the request  |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")        |
| result    | string | Hash of the latest BEEFY-finalized block |

## Use Cases

* **Bridge Finality**: Read BEEFY finality for cross-chain bridges
* **Trust-Minimized Bridges**: Anchor bridges to BEEFY proofs
* **Monitoring**: Track BEEFY finalization progress
* **Interoperability**: Support Ethereum-facing light clients

## Error Handling

| Error Code | Message          | Description                                            |
| ---------- | ---------------- | ------------------------------------------------------ |
| -32602     | Invalid params   | A parameter is missing or has the wrong type or format |
| -32601     | Method not found | The method is not available on this endpoint           |
| -32603     | Internal error   | The node failed to process the request                 |

## Library Integration

The idiomatic way to call Polkadot RPC is the Polkadot.js API (`@polkadot/api`), which connects over WebSocket and exposes the method as `api.rpc.beefy.getFinalizedHead`.

{% code title="polkadot-js.js" %}
```javascript
import { ApiPromise, WsProvider } from '@polkadot/api';

const provider = new WsProvider('wss://go.getblock.io/<ACCESS-TOKEN>/');
const api = await ApiPromise.create({ provider });

const result = await api.rpc.beefy.getFinalizedHead();
console.log(result.toHuman());
```
{% endcode %}
