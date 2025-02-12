# account \\\[POST\\]

#### Example

`POST /account/get`

#### Request

```
curl --location --request POST 'https://near.getblock.io/explorer/account/get' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'x-api-key: YOUR-API-KEY' \
--data-raw '{
  "address": "aurora"
}'
```

#### Response

```
{
  "address": "aurora",
  "balance": {
    "USD": 88.55,
    "yoctoNEAR": "51451903615609279009283504"
  },
  "created_at": 1601591231895000000,
  "creation_tx": "AqZs5TUgpCFTfzJQAyGkvUzSpGBmiybPj1qxentDeHuW",
  "stake": "51451903615609279009283504",
  "storage_used": 4018,
  "tx_in": 1246,
  "tx_out": 10
}
```
