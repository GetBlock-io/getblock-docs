# getblockbylimitnext

This endpoint returns a range of blocks between a start height (inclusive) and an end height (exclusive), in one call.

{% hint style="info" %}
This is a Solidity-node endpoint. It returns only confirmed, irreversible data, so it is the correct interface for balance and payment verification. The Fullnode serves the same operation at `https://go.getblock.io/<ACCESS-TOKEN>/wallet/getblockbylimitnext` over the latest, possibly unconfirmed state.
{% endhint %}

## Parameters

| Parameter | Type    | Required | Description                                |
| --------- | ------- | -------- | ------------------------------------------ |
| startNum  | integer | Yes      | First block height in the range, inclusive |
| endNum    | integer | Yes      | End block height, exclusive                |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/walletsolidity/getblockbylimitnext' \
--header 'Content-Type: application/json' \
--data-raw '{
  "startNum": 68000000,
  "endNum": 68000005
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch(
    'https://go.getblock.io/<ACCESS-TOKEN>/walletsolidity/getblockbylimitnext',
    {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({"startNum": 68000000, "endNum": 68000005})
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
    'https://go.getblock.io/<ACCESS-TOKEN>/walletsolidity/getblockbylimitnext',
    headers={'Content-Type': 'application/json'},
    json={"startNum": 68000000, "endNum": 68000005}
)

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
  "block": [
    {
      "blockID": "0000000002f3a5b0f6d2e6c9a1b4e7f0a3b6c9d2e5f8a1b4c7d0e3f6a9b2c5d8",
      "block_header": {
        "raw_data": {
          "number": 68000000,
          "timestamp": 1719400000000
        }
      }
    }
  ]
}
```

## Response Parameters

| Field | Type  | Description                                                      |
| ----- | ----- | ---------------------------------------------------------------- |
| block | array | Blocks in the requested range, each with header and transactions |

## Use Cases

* **Batch Indexing**: Fetch a window of blocks in one request
* **Backfill**: Catch up a store over a range of heights
* **Analytics**: Read a contiguous span of blocks for analysis
* **Reorg Windows**: Re-scan a recent range after a rollback

## Error Handling

| HTTP Status | Message        | Description                                                                            |
| ----------- | -------------- | -------------------------------------------------------------------------------------- |
| 200         | OK             | The request succeeded; some TRON errors are returned in the 200 body as an Error field |
| 400         | Bad request    | The request body is malformed or a required field is missing                           |
| 500         | Internal error | The node failed to process the request                                                 |
