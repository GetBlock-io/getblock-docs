# blocks\_blockId

This endpoint returns a block by its number or hash, with decoded extrinsics and events. It is the historical counterpart of `/blocks/head`.

{% hint style="info" %}
On GetBlock's unified endpoint, Asset Hub is the default network. To call this endpoint against the Relaychain, add an `/rc` prefix to the path (for example, `/rc/blocks/{blockId}`).
{% endhint %}

## Endpoint

```http
GET /blocks/{blockId}
```

## Path Parameters

| Parameter | Type   | Description                |
| --------- | ------ | -------------------------- |
| blockId   | string | Block number or block hash |

## Query Parameters

| Parameter     | Type    | Required | Description                     |
| ------------- | ------- | -------- | ------------------------------- |
| eventDocs     | boolean | No       | Include event documentation     |
| extrinsicDocs | boolean | No       | Include extrinsic documentation |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location 'https://go.getblock.io/<ACCESS-TOKEN>/blocks/6754362'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch('https://go.getblock.io/<ACCESS-TOKEN>/blocks/6754362');
const data = await response.json();
console.log(data);
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

response = requests.get('https://go.getblock.io/<ACCESS-TOKEN>/blocks/6754362')
print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
  "number": "6754362",
  "hash": "0x255bc00927df8d33d561792635cbc6bde480a0a505eef5ff28630ece3fc15b32",
  "authorId": "15oF4uVJwmo4TdGW7VfQxNLavjCXviqxT9S1MgbjMNHr6Sp5",
  "extrinsics": [
    {
      "method": {
        "pallet": "balances",
        "method": "transferKeepAlive"
      },
      "signature": {
        "signer": {
          "id": "15oF4uVJwmo4TdGW7VfQxNLavjCXviqxT9S1MgbjMNHr6Sp5"
        }
      },
      "nonce": "1233",
      "success": true,
      "events": [
        {
          "method": {
            "pallet": "balances",
            "method": "Transfer"
          }
        }
      ]
    }
  ],
  "onInitialize": {
    "events": []
  },
  "onFinalize": {
    "events": []
  }
}
```

## Response Fields

| Field                 | Type    | Description                           |
| --------------------- | ------- | ------------------------------------- |
| number                | string  | Block number                          |
| hash                  | string  | Block hash                            |
| extrinsics\[].method  | object  | The pallet and call of each extrinsic |
| extrinsics\[].success | boolean | Whether the extrinsic succeeded       |
| extrinsics\[].events  | array   | Events emitted by the extrinsic       |

## Use Cases

* **Historical Lookups**: Fetch a past block by number or hash
* **Transaction Receipts**: Read an extrinsic's success and events
* **Auditing**: Inspect the calls in a historical block
* **Analytics**: Aggregate on-chain activity by block
