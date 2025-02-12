# tx\_get \\\[POST\\]

#### Example

`POST /tx/get`

#### Request

```
curl --location --request POST 'https://near.getblock.io/explorer/tx/get' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'x-api-key: YOUR-API-KEY' \
--data-raw '{
  "hash": "2np4trodC1iFS4Zg2oVvcGwyMfrDEmaiUm48qyQUDjeU"
}'
```

#### Response

```
{
  "actions": [
    "Transfer"
  ],
  "block_hash": "39F3yvyJJAaZ3RPHVVWNKeqrBRTKq9iXbQCAqSqtFQGq",
  "block_height": 67909065,
  "convert_to_receipt": {
    "gas_burned": "424555062500",
    "tokens_burned": "42455506250000000000"
  },
  "created_at": 1655460557281346000,
  "deposit": "50000000000000000000000",
  "fee": "84911012500000000000",
  "gas_attached": "1072292687500",
  "gas_price": "100000000",
  "gas_used": "1072292687500",
  "hash": "2np4trodC1iFS4Zg2oVvcGwyMfrDEmaiUm48qyQUDjeU",
  "receipt_ids": [
    "B6r7SAqutgMNmtsD8r25ajq8MMykhx2eVLnTcTdWcqiG",
    "H1bC1XWE2y14dgMzGtqMdToAwSLYUDM1ybveqWzFjAyz"
  ],
  "receipts": [
    {
      "actions": [
        {
          "AddKey": {
            "access_key": {
              "nonce": 0,
              "permission": "string"
            },
            "public_key": "string"
          },
          "CreateAccount": "string",
          "DeleteAccount": {
            "beneficiary_id": "string"
          },
          "DeleteKey": {
            "public_key": "string"
          },
          "DeployContract": {
            "code": "string"
          },
          "FunctionCall": {
            "args": "string",
            "deposit": "string",
            "gas": "string",
            "method_name": "string"
          },
          "Stake": {
            "public_key": "string",
            "stake": "string"
          },
          "Transfer": {
            "amount": "string"
          }
        }
      ],
      "block_hash": "string",
      "block_height": 0,
      "child": [
        "string"
      ],
      "executor_id": "string",
      "gas_used": "string",
      "id": "string",
      "logs": [
        "string"
      ],
      "predecessor_id": "string",
      "result": "string",
      "successful": true,
      "tokens_burnt": "string",
      "tx_hash": "string"
    }
  ],
  "receiver_id": "relay.aurora",
  "signer_id": "sweat_welcome.near",
  "status": "string",
  "successful": true
}
```
