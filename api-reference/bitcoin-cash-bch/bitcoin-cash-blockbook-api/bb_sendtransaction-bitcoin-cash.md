---
description: >-
  Example code for the bb_sendTransaction JSON-RPC method. Complete guide on how
  to use the bb_sendTransaction JSON-RPC method in the GetBlock Web3
  documentation.
---

# bb\_sendTransaction - Bitcoin Cash

This method broadcasts a signed, serialized transaction to the Bitcoin Cash network through the indexer's backend node and returns the transaction id on acceptance.

## Parameters

| Parameter | Type   | Required | Description                                              |
| --------- | ------ | -------- | -------------------------------------------------------- |
| hex       | string | Yes      | The raw signed transaction hex, sent in the request body |

## Request

{% tabs %}
{% tab title="cURL (REST)" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/sendtx/' \
--header 'Content-Type: text/plain' \
--data-raw '01000000000101e17e03d21d051aa2bd9d336c3ac0693cfa92ce71592ceec521b1c48019ff77a10100000017160014...signed...raw...tx...hex...00000000'
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
        "01000000000101e17e03d21d051aa2bd9d336c3ac0693cfa92ce71592ceec521b1c48019ff77a10100000017160014...signed...raw...tx...hex...00000000"
    ],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/sendtx/',
    {
        method: 'POST',
        headers: { 'Content-Type': 'text/plain' },
        body: '01000000000101e17e03d21d051aa2bd9d336c3ac0693cfa92ce71592ceec521b1c48019ff77a10100000017160014...signed...raw...tx...hex...00000000'
    }
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

response = requests.post(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/sendtx/',
    headers={'Content-Type': 'text/plain'},
    data='01000000000101e17e03d21d051aa2bd9d336c3ac0693cfa92ce71592ceec521b1c48019ff77a10100000017160014...signed...raw...tx...hex...00000000'
)

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "result": "10b54fd708ab2e5703979b4ba27ca0339882abc2062e77fbe51e625203a49642"
}
```

## Response Parameters

| Field  | Type   | Description                                    |
| ------ | ------ | ---------------------------------------------- |
| result | string | The transaction id of the accepted transaction |

## Use Cases

* **Payment Broadcast**: Submit a signed transaction through the indexer endpoint
* **Wallet Backends**: Broadcast transactions built and signed client-side
* **Unified Integration**: Broadcast through the same endpoint used for queries
* **Retry Flows**: Resubmit a dropped transaction from stored raw hex

## Error Handling

| Error Code   | Message              | Description                                    |
| ------------ | -------------------- | ---------------------------------------------- |
| 400 / -32602 | Bad request          | The transaction hex could not be decoded       |
| -26 / -32603 | Transaction rejected | The transaction failed node mempool acceptance |
| 500 / -32603 | Internal error       | The node failed to broadcast the transaction   |
