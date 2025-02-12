---
description: >-
  Example code for the /tryLocateSourceTx json-rpc method. Сomplete guide on how
  to use /tryLocateSourceTx json-rpc in GetBlock.io Web3 documentation.
---

# /tryLocateSourceTx - The Open Network (TON)

#### Parameters

`source` - query

string, required

`destination` - query

string, required

`created_lt` - query

integer, required

#### Request

```java
curl --location --request GET 'https://ton.getblock.io/mainnet//tryLocateSourceTx?source=EQC1CQH3A1RA5Vmk30ayYByoVfF-m44MKiQiWhOJWXLtPCn3&destination=EQDXZ2c5LnA12Eum-DlguTmfYkMOvNeFCh4rBD0tgmwjcFI-&created_lt=38633831000014' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json'
```

#### Response

```java
{
    "ok": true,
    "result": {
        "@type": "raw.transaction",
        "data": "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",
        "fee": "14474247",
        "in_msg": {
            "@type": "raw.message",
            "body_hash": "xA17WSUIz19xbNc5D8TQdHJF2uLdZxXRHN45DBYyvL0=",
            "created_lt": "38633831000011",
            "destination": "EQC1CQH3A1RA5Vmk30ayYByoVfF-m44MKiQiWhOJWXLtPCn3",
            "fwd_fee": "7430057",
            "ihr_fee": "0",
            "message": "F41FGSGa8cHnBZnwMU9xmAFwpQvZtAEujcBwTQ0HHMsfab5zldOOYZzhK3XMorR6aQA12dnOS5wNdhLpvg5YLk5n2JDDrzXhQoeKwQ9LYJsI3BAYgzzw",
            "msg_data": {
                "@type": "msg.dataRaw",
                "body": "te6cckEBAQEAWQAArReNRRkhmvHB5wWZ8DFPcZgBcKUL2bQBLo3AcE0NBxzLH2m+c5XTjmGc4St1zKK0emkANdnZzkucDXYS6b4OWC5OZ9iQw6814UKHisEPS2CbCNwQGIM88bHNq3E=",
                "init_state": "te6cckECFQEAA9EAAgE0AgEBhwgBrs7Oclzga7CXTfBywXJzPsSGHXmvChQ8Vgh6WwTYRuEAHKcE7bfLAfL8KBqtj00r0VPPTIOwbzY+1xtJNCAahfCgAgEU/wD0pBP0vPLICwMCAWIEBQICywYHABug9gXaiaH0AfSB9IGpowIBzggJAgFYDA0C9wgxwCSXwTgAdDTAwFxsJUTXwPwHeD6QPpAMfoAMXHXIfoAMfoAMHOptAAC0x8B2zxbMjQ0NCSCEA+KfqW6mjBsIjZeMRAj8BrgJIIQF41FGbqbMGwiXjIQJEMA8BvgN1s2ghBZXwe8up8CcbDy0sBQI7ry4sYB8BzgXwWAKCwARPpEMMAA8uFNgAFyAT/gzIG6VMICx+DPeIG7y0prQ0wcx0//T//QE0wfUMND6APoA+gD6APoA+gAwAAiED/LwAgFYDg8CAUgTFAH3AXTPwEB+gD6QCHwAe1E0PoA+kD6QNTRUTahUizHBfLiwSrC//LiwlQ0QnBUIBNUFAPIUAT6AljPFgHPFszJIsjLARL0APQAywDJIHAB+QB0yMsCEsoHy//J0AT6QPQEMfoAINdJwgDy4sTIgBgBywVQB88WcPoCdwHLa4BAC8ztRND6APpA+kDU0QrTPwEB+gBRUaAF+kD6QFNdxwVUc29wVCATVBQDyFAE+gJYzxYBzxbMySLIywES9AD0AMsAyXAB+QB0yMsCEsoHy//J0FAPxwUesfLiwwz6AFHKoSm2CBmhUAegGKEmkmxV4w0l1wsBwwAhwgCwgERIAqhPMyIIQF41FGVgKAssfyz9QB/oCIs8WUAbPFiX6AlADzxbJUAXMI5FykXHiUAeoE6AIqgBQBKAXoBS88uLFAcmAQPsAQwDIUAT6AljPFgHPFszJ7VQAclJpoBihyIIQc2LQnCkCyx/LP1AH+gJQBM8WUAfPFsnIgBABywUnzxZQBPoCcQHLahPMyXH7AFBCEwB0jiPIgBABywVQBs8WUAX6AnABy2qCENUydttYBQLLH8s/yXL7AJJbM+JAA8hQBPoCWM8WAc8WzMntVADrO1E0PoA+kD6QNTRBdM/AQH6ACHCAPLiwvpA9AQB0NOf0QHRUWKhUljHBfLiwSbC//LiwsiCEHvdl95YBALLH8s/AfoCI88WAc8WE8ufyciAGAHLBSPPFnD6AnEBy2rMyYBA+wBAE8hQBPoCWM8WAc8WzMntVIACHIAg1yHtRND6APpA+kDU0QTTHwGEDyGCEBeNRRm6AoIQe92X3roSsfL00z8BMPoAMBOgUCPIUAT6AljPFgHPFszJ7VSBXNpsI"
            },
            "source": "EQCBsjUANh1o0D054rEvP-UKd75FP5BSHeuca_J2PnXlj4h3",
            "value": "134792500"
        },
        "other_fee": "14129000",
        "out_msgs": [
            {
                "@type": "raw.message",
                "body_hash": "7o7+mXazXzO/gmvAfXXomeBLFDM6g+q7CICuIxTCW5I=",
                "created_lt": "38633831000014",
                "destination": "EQDXZ2c5LnA12Eum-DlguTmfYkMOvNeFCh4rBD0tgmwjcFI-",
                "fwd_fee": "666672",
                "ihr_fee": "0",
                "message": "1TJ22yGa8cHnBZnw",
                "msg_data": {
                    "@type": "msg.dataRaw",
                    "body": "te6cckEBAQEADgAAGNUydtshmvHB5wWZ8C7L/gM=",
                    "init_state": ""
                },
                "source": "EQC1CQH3A1RA5Vmk30ayYByoVfF-m44MKiQiWhOJWXLtPCn3",
                "value": "4833915"
            }
        ],
        "storage_fee": "345247",
        "transaction_id": {
            "@type": "internal.transactionId",
            "hash": "uGMCii8XMyIfna45OItWSZuvYbtoeFVQtdNYFSDK3XU=",
            "lt": "38633831000012"
        },
        "utime": 1687261621
    }
}
```
