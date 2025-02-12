# account\_list \\\[POST\\]

#### Example

`POST /account/list`

#### Request

```
curl --location --request POST 'https://near.getblock.io/explorer/account/list' \
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
[{
  "account": "alice.near",
  "balance": "51903615609279009283",
  "created_at": 1601591231895000000,
  "creation_tx": "AqZs5TUgpCFTfzJQAyGkvUzSpGBmiybPj1qxentDeHuW",
  "stake": "51451903615609279009283504",
  "transaction_count": 1050
}]
```
