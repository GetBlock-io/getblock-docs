# dot\_state\_getruntimeversion

This method returns the runtime version at a given block, including the spec name, spec version, and the supported runtime API versions. Clients use it to detect runtime upgrades.

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
--data-raw '{"jsonrpc": "2.0", "method": "state_getRuntimeVersion", "params": [], "id": "getblock.io"}'
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
        method: 'state_getRuntimeVersion',
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
        'method': 'state_getRuntimeVersion',
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
    "result": {
        "specName": "polkadot",
        "implName": "parity-polkadot",
        "authoringVersion": 0,
        "specVersion": 1002000,
        "implVersion": 0,
        "apis": [
            [
                "0xdf6acb689907609b",
                5
            ],
            [
                "0x37e397fc7c91f5e4",
                2
            ]
        ],
        "transactionVersion": 26,
        "stateVersion": 1
    }
}
```

## Response Parameters

| Parameter | Type   | Description                             |
| --------- | ------ | --------------------------------------- |
| id        | string | Request identifier matching the request |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")       |
| result    | object | Runtime version object                  |

### Result Object

| Field              | Type    | Description                                           |
| ------------------ | ------- | ----------------------------------------------------- |
| specName           | string  | Runtime specification name (polkadot)                 |
| implName           | string  | Runtime implementation name                           |
| specVersion        | integer | Runtime spec version; increments on a runtime upgrade |
| transactionVersion | integer | Version of the transaction format                     |
| apis               | array   | Supported runtime API identifiers and their versions  |

## Use Cases

* **Upgrade Detection**: Detect runtime upgrades by watching specVersion
* **Transaction Signing**: Read transactionVersion when constructing extrinsics
* **Compatibility Checks**: Confirm supported runtime API versions
* **Tooling**: Configure SDKs against the current runtime

## Error Handling

| Error Code | Message          | Description                                            |
| ---------- | ---------------- | ------------------------------------------------------ |
| -32602     | Invalid params   | A parameter is missing or has the wrong type or format |
| -32601     | Method not found | The method is not available on this endpoint           |
| -32603     | Internal error   | The node failed to process the request                 |

## Library Integration

The idiomatic way to call Polkadot RPC is the Polkadot.js API (`@polkadot/api`), which connects over WebSocket and exposes the method as `api.rpc.state.getRuntimeVersion`.

{% code title="polkadot-js.js" %}
```javascript
import { ApiPromise, WsProvider } from '@polkadot/api';

const provider = new WsProvider('wss://go.getblock.io/<ACCESS-TOKEN>/');
const api = await ApiPromise.create({ provider });

const result = await api.rpc.state.getRuntimeVersion();
console.log(result.toHuman());
```
{% endcode %}
