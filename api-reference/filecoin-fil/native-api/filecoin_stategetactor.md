---
description: >-
  Example code for the Filecoin.StateGetActor JSON RPC method. Complete guide on
  how to use Filecoin.StateGetActor JSON RPC in GetBlock Web3 documentation.
---

# Filecoin.StateGetActor - Filecoin

This method returns the on-chain actor state for an address at a given tipset: its code CID, state root CID, nonce, and balance in attoFIL.

{% hint style="info" %}
Many state methods take a tipset key as their final parameter: an array of block CIDs identifying the tipset to read. Pass an empty array `[]` to read against the current chain head.
{% endhint %}

## Parameters

| Parameter | Type   | Required | Description                                             |
| --------- | ------ | -------- | ------------------------------------------------------- |
| address   | string | Yes      | The address to read (f0/f1/f2/f3/f4)                    |
| tipsetKey | array  | Yes      | Tipset key to read from; empty array for the chain head |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0", "method": "Filecoin.StateGetActor", "params": ["f1ne72cbn6r55wea7ifjv4ypyti7t2df5dumsjhzq", []], "id": "getblock.io"}'
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
        method: 'Filecoin.StateGetActor',
        params: ["f1ne72cbn6r55wea7ifjv4ypyti7t2df5dumsjhzq", []],
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
        'method': 'Filecoin.StateGetActor',
        'params': ["f1ne72cbn6r55wea7ifjv4ypyti7t2df5dumsjhzq", []],
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
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "Code": {
            "/": "bafk2bzacedu...account"
        },
        "Head": {
            "/": "bafy2bzaceaglcpzhd5gfrzdyt7ce3e5asnbfz3s3stbqyxniziny5snewbpbg"
        },
        "Nonce": 42,
        "Balance": "5000000000000000000"
    }
}
```

## Response Parameters

| Field   | Type    | Description                               |
| ------- | ------- | ----------------------------------------- |
| Code    | object  | CID of the actor's code (its type)        |
| Head    | object  | CID of the actor's current state root     |
| Nonce   | integer | Actor nonce (next expected message nonce) |
| Balance | string  | Actor balance in attoFIL                  |

## Use Cases

* **Balance Reads**: Read an actor's FIL balance
* **Nonce Lookups**: Read the next nonce for message construction
* **Actor Typing**: Identify an actor by its code CID
* **State Access**: Get the state root to read deeper actor state

## Error Handling

| Error Code | Message         | Description                                                 |
| ---------- | --------------- | ----------------------------------------------------------- |
| -32602     | Invalid params  | A parameter is missing or has the wrong type or format      |
| 1          | Actor not found | The address or actor does not exist in the requested tipset |
| -32603     | Internal error  | The node failed to process the request                      |
