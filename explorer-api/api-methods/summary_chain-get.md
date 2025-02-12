# summary\_chain \\\[GET\\]

#### Example

`GET /summary/chain`

#### Request

```
curl --location --request GET 'https://near.getblock.io/explorer/summary/chain' \
--header 'Accept: application/json' \
--header 'x-api-key: YOUR-API-KEY'
```

#### Response

```
{
  "accounts": 22633466,
  "blocks": 22817487,
  "burnt_24h": 5677.477799699645,
  "epoch_end": 1670459242,
  "median_gas_price": 7.833964225336999e-12,
  "node_validating": 134,
  "time_between_blocks_ms": 4000,
  "total_supply": "1110474552246624165666503762734209",
  "transactions": 95482007,
  "tx_per_second": 4.159279697085327
}
```
