---
description: >-
  Example code for the bb_sendTransaction JSON-RPC method. Complete guide on how
  to use the bb_sendTransaction JSON-RPC method in the GetBlock Web3
  documentation.
---

# bb\_sendTransaction - Bitcoin

This method broadcasts a signed, serialized transaction to the network through the indexer and returns its transaction id.

## Parameters

| Parameter | Type   | Required | Description                              |
| --------- | ------ | -------- | ---------------------------------------- |
| hex       | string | Yes      | The signed transaction serialized as hex |

## Request

{% tabs %}
{% tab title="cURL (REST)" %}
{% code overflow="wrap" %}
```bash
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/sendtx/0200000001...signed...00000000'
```
{% endcode %}
{% endtab %}

{% tab title="cURL (JSON-RPC)" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "bb_sendTransaction",
    "params": [
        "0200000001...signed...00000000"
    ],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/sendtx/0200000001...signed...00000000');
const data = await response.json();
console.log(data);
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/sendtx/0200000001...signed...00000000')
print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
  "result": "4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b"
}
```

## Response Parameters

| Field  | Type   | Description                                     |
| ------ | ------ | ----------------------------------------------- |
| result | string | The transaction ID of the broadcast transaction |

## Use Cases

* **Transaction Broadcast**: Submit a signed transaction over Blockbook
* **Wallet Backends**: Broadcast without a full node
* **Custody Flows**: Broadcast externally signed transactions
* **Automation**: Send transactions from a pipeline

## Error Handling

| HTTP Status | Message      | Description                               |
| ----------- | ------------ | ----------------------------------------- |
| 400         | Bad request  | A path or query parameter is malformed    |
| 403         | Forbidden    | Missing or invalid ACCESS-TOKEN           |
| 404         | Not found    | The requested resource does not exist     |
| 500         | Server error | The indexer failed to process the request |
