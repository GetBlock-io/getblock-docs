# block\_list \\\[POST\\]

#### Example

`POST /block/list`

#### Request

```
curl --location --request POST 'https://near.getblock.io/explorer/block/list' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'x-api-key: YOUR-API-KEY' \
--data-raw '{
  "count": 100,
  "offset": 0
}'
```

#### Response

```
[
  {
    "author": "bzam6yjpnfnxsdmjf6pw.poolv1.near",
    "created_at": 1655390203442722000,
    "gas_price": "100000000",
    "gas_used": "65762634519528",
    "hash": "F98BtBgQKTYD9VB36ArLLVkt6rhepdis3MUHyHnhSvvo",
    "height": 67851339,
    "receipts": 12,
    "tx_count": 10
  }
]
```
