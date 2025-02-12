# account\_list\_by-tx-count \\\[POST\\]

#### Example

`POST /account/list/by-tx-count`

#### Request

```
curl --location --request POST 'https://near.getblock.io/explorer/account/list/by-tx-count' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'x-api-key: YOUR-API-KEY' \
--data-raw '{
  "count": 25,
  "offset": 0
}'
```

#### Response

```
[
  {
    "account": "alice.near",
    "tx_count": 1080
  }
]
```
