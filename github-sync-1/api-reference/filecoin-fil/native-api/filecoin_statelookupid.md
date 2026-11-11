---
description: >-
  Example code for the Filecoin.StateLookupID JSON RPC method. Complete guide on
  how to use Filecoin.StateLookupID JSON RPC in GetBlock Web3 documentation.
---

# Filecoin.StateLookupID - Filecoin

This method returns the short ID address (f0...) for a given address at a tipset. ID addresses are compact and stable, and are used internally by the chain.

{% hint style="info" %}
Many state methods take a tipset key as their final parameter: an array of block CIDs identifying the tipset to read. Pass an empty array `[]` to read against the current chain head.
{% endhint %}

## Parameters

| Parameter | Type   | Required | Description                                             |
| --------- | ------ | -------- | ------------------------------------------------------- |
| address   | string | Yes      | The address to resolve (f1/f2/f3/f4)                    |
| tipsetKey | array  | Yes      | Tipset key to read from; empty array for the chain head |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0", "method": "Filecoin.StateLookupID", "params": ["f1ne72cbn6r55wea7ifjv4ypyti7t2df5dumsjhzq", []], "id": "getblock.io"}'
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
        method: 'Filecoin.StateLookupID',
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
        'method': 'Filecoin.StateLookupID',
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
    "result": "f0123456"
}
```

## Response Parameters

| Field  | Type   | Description                                  |
| ------ | ------ | -------------------------------------------- |
| result | string | The ID address (f0...) for the input address |

## Use Cases

* **Address Normalization**: Resolve any address to its ID form
* **Indexing Keys**: Use the stable ID address as a key
* **Deduplication**: Detect that two addresses are the same actor
* **State Queries**: Use ID addresses in downstream calls

## Error Handling

| Error Code | Message         | Description                                                 |
| ---------- | --------------- | ----------------------------------------------------------- |
| -32602     | Invalid params  | A parameter is missing or has the wrong type or format      |
| 1          | Actor not found | The address or actor does not exist in the requested tipset |
| -32603     | Internal error  | The node failed to process the request                      |
