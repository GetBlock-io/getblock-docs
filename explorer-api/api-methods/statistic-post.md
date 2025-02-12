# statistic \\\[POST\\]

#### Example

`POST /statistic`

#### Request

```
curl --location --request POST 'https://near.getblock.io/explorer/statistic' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'x-api-key: YOUR-API-KEY' \
--data-raw '{
  "days": 6,
  "type": "tx"
}'
```

#### Response

```
[
  {
    "date": "2022-12-07",
    "value": {
      "data": 183813,
      "price": 1.7276842858336163
    }
  }
]
```
