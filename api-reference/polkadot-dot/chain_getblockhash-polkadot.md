# chain\_getblockhash - Polkadot

This method returns the hash of the block at a given block number. If no number is supplied, the hash of the latest block is returned.

## Parameters

| Parameter   | Type              | Required | Description                                                      |
| ----------- | ----------------- | -------- | ---------------------------------------------------------------- |
| blockNumber | integer or string | No       | Block number, as an integer or hex. Defaults to the latest block |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0", "method": "chain_getBlockHash", "params": [6754362], "id": "getblock.io"}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" overflow="wrap" %}
```javascript
const response = await fetch('https://shared.eu-central-1.getblock.io/<<ACCESS-TOKEN>/', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
        jsonrpc: '2.0',
        method: 'chain_getBlockHash',
        params: [6754362],
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
    'https://shared.eu-central-1.getblock.io/<<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'chain_getBlockHash',
        'params': [6754362],
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

| Parameter | Type   | Description                             |
| --------- | ------ | --------------------------------------- |
| id        | string | Request identifier matching the request |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")       |
| result    | string | The 32-byte block hash                  |

## Use Cases

* **Hash Resolution**: Resolve a block number to its hash for other calls
* **Pagination**: Walk the chain by number, resolving each hash
* **Snapshotting**: Pin a block hash for consistent state queries
* **Explorer Links**: Map a height to a linkable block hash

## Error Handling

| Error Code | Message          | Description                                            |
| ---------- | ---------------- | ------------------------------------------------------ |
| -32602     | Invalid params   | A parameter is missing or has the wrong type or format |
| -32601     | Method not found | The method is not available on this endpoint           |
| -32603     | Internal error   | The node failed to process the request                 |

## Library Integration

The idiomatic way to call Polkadot RPC is the Polkadot.js API (`@polkadot/api`), which connects over WebSocket and exposes the method as `api.rpc.chain.getBlockHash`.

{% code title="polkadot-js.js" overflow="wrap" %}
```javascript
import { ApiPromise, WsProvider } from '@polkadot/api';

const provider = new WsProvider('wss://shared.eu-central-1.getblock.io/<<ACCESS-TOKEN>/');
const api = await ApiPromise.create({ provider });

const result = await api.rpc.chain.getBlockHash(6754362);
console.log(result.toHuman());
```
{% endcode %}
