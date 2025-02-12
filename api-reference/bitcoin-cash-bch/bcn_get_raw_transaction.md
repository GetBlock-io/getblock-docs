---
description: >-
  Example code for the get_raw_transaction json-rpc method. Сomplete guide on
  how to use get_raw_transaction json-rpc in GetBlock.io Web3 documentation.
---

# get\_raw\_transaction - Bitcoin Cash

#### Parameters

`hash` - string

Hash of a transaction being requested.

`need_signatures` - bool

Optional, default=false

If true, adds Signature objects to the response.

#### Request

```java
curl --location --request POST 'https://bcn.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "get_raw_transaction",
"params": {"hash": "89eb3525d61f10dc868e0b23151b27aee14808c75417dc43dff7d22aed1f7f16"},
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "mixed_outputs": [],
        "mixed_public_keys": [],
        "raw_transaction": {
            "extra": "01db4b1131bc6fb9c96e7f84291aff6947663887e5ea1042a3d11fac9592b2839a0403a08d06",
            "inputs": [
                {
                    "height": 2398925,
                    "type": "coinbase"
                }
            ],
            "outputs": [
                {
                    "amount": 279000,
                    "public_key": "09e5e9735184180875736ccf29f61b579b34accb79a86bd87a4135412122cf3c",
                    "type": "key"
                },
                {
                    "amount": 202,
                    "public_key": "57c2ff72ebbcf3e7a423ac9cc7febeee766e1c7e4870758ee431269d3cab5867",
                    "type": "key"
                },
                {
                    "amount": 3000000,
                    "public_key": "bfd0091cd7ce95536891b83aca07483d41cdad0a5aa18bae39676daac544f1ed",
                    "type": "key"
                },
                {
                    "amount": 80000000,
                    "public_key": "7c3d28c0b151bb812f9d198cb938d492887c9a2b5dbee18da36b46ab422d5681",
                    "type": "key"
                },
                {
                    "amount": 900000000,
                    "public_key": "af8c54a3d39abca4f09cf7556fc2d5b6ad5183e60abcdf58a6db1516c86b95a2",
                    "type": "key"
                },
                {
                    "amount": 7000000000,
                    "public_key": "8e8450f60036c828f83d8a2f70800e2029a7e982b4564c608e94f7e184ac59db",
                    "type": "key"
                }
            ],
            "unlock_block_or_timestamp": 0,
            "version": 1
        },
        "transaction": {
            "amount": 7983279202,
            "anonymity": 0,
            "binary_size": 269,
            "block_hash": "0012daca5694fd5f228e61144160a0f2c2b9920e467012930b3a67393c607988",
            "block_height": 2398925,
            "coinbase": true,
            "extra": "01db4b1131bc6fb9c96e7f84291aff6947663887e5ea1042a3d11fac9592b2839a0403a08d06",
            "fee": 0,
            "hash": "89eb3525d61f10dc868e0b23151b27aee14808c75417dc43dff7d22aed1f7f16",
            "inputs_hash": "6ec2c9006d228c9aaf5d11798e74159f7612341c9b9a18fe8adf85b88559d80c",
            "prefix_hash": "89eb3525d61f10dc868e0b23151b27aee14808c75417dc43dff7d22aed1f7f16",
            "public_key": "db4b1131bc6fb9c96e7f84291aff6947663887e5ea1042a3d11fac9592b2839a",
            "size": 269,
            "timestamp": 1631266587,
            "unlock_block_or_timestamp": 0,
            "unlock_time": 0
        }
    }
}
```
