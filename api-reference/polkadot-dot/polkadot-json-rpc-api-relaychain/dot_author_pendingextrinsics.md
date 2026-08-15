# dot\_author\_pendingextrinsics

This method returns the extrinsics currently in the node's transaction pool that are waiting to be included in a block.

## Parameters

This method does not accept any parameters.

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0", "method": "author_pendingExtrinsics", "params": [], "id": "getblock.io"}'
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
        method: 'author_pendingExtrinsics',
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
        'method': 'author_pendingExtrinsics',
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
    "result": [
        "0x4d028400d43593c715fdd31c61141abd04a99fd6822c8558854ccde39a5684e7a56da27d01..."
    ]
}
```

## Response Parameters

| Parameter | Type   | Description                                                     |
| --------- | ------ | --------------------------------------------------------------- |
| id        | string | Request identifier matching the request                         |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                               |
| result    | array  | Array of pending extrinsics, each SCALE-encoded and hex-encoded |

## Use Cases

* **Mempool Inspection**: Read transactions waiting for inclusion
* **Fee Markets**: Observe pending demand for block space
* **Resubmission Logic**: Detect whether a transaction is still pending
* **Monitoring**: Track transaction pool depth

## Error Handling

| Error Code | Message          | Description                                            |
| ---------- | ---------------- | ------------------------------------------------------ |
| -32602     | Invalid params   | A parameter is missing or has the wrong type or format |
| -32601     | Method not found | The method is not available on this endpoint           |
| -32603     | Internal error   | The node failed to process the request                 |

## Library Integration

The idiomatic way to call Polkadot RPC is the Polkadot.js API (`@polkadot/api`), which connects over WebSocket and exposes the method as `api.rpc.author.pendingExtrinsics`.

{% code title="polkadot-js.js" %}
```javascript
import { ApiPromise, WsProvider } from '@polkadot/api';

const provider = new WsProvider('wss://go.getblock.io/<ACCESS-TOKEN>/');
const api = await ApiPromise.create({ provider });

const result = await api.rpc.author.pendingExtrinsics();
console.log(result.toHuman());
```
{% endcode %}
