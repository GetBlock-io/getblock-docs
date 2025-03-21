---
description: >-
  Example code for the getrawmempool json-rpc method. Сomplete guide on how to
  use getrawmempool json-rpc in GetBlock.io Web3 documentation.
---

# getrawmempool - Bitcoin

#### Parameters

`verbose` - boolean, optional, default=false

True for a json object, false for array of transaction ids

`mempool_sequence` - boolean, optional, default=false

If verbose=false, returns a json object with transaction list and mempool sequence number attached.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/v1/mainnet/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "getrawmempool",
"params": [null, null],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": [
        "d7d42e8c0ee7721dc5e417fb746c4ac404c7f2cd39e1343b3aa03741fa777782",
        "27b8644cb6d365e9f57a40d01ce3599d1cfb4f50ffdfaf927bcd1277c96dae12",
        "fc5e53103d5c9d83e76d8f7f133fb2a519efa572e451392e73a90629895f89f3",
        "442723267ed7f1b39b66316c8144aef9f3a8fcf30d53ff6750ed34f69e11aa9a",
        "fecba20b5b84bcb90275e02eb1aeb96c48d0e1b3ae3eb2856304e63c46c2459e",
        "d496dc633a81455f07a1ac0f587ecbd2ea7745d141b48882c15ee5b64932f260",
        "45fabcc0e63a0830bd2ca03aa9ffca5827e9201e148e5f5a04d42af53e20fdda",
        "75d8a77fd06818ae213b07d545d238a1f1a453d8152473e560fe0a251b876c04",
        "81dcef9211fb8b1091b4d040c7c32347350af3871909df7fb1dcebd36cb16820",
        "b1a214a33b1fcbd61cf0c496b7b0b0cd52c57fcfcea9f00118e41e2285a6973a",
        "8827d4c3f9edc65d5550b5700b72c70d26588c6eb6caee05d0f3142c4c90bdcd",
        "a6cb413669eef903ebef42f249b7a775c9a429e6847129d7a8569f8ca3c00856",
        "40fd6c80d2977a6a7b9f7f6e4d623eae2afb93e666638122ad7b610989ede995",
        "a4aef4dd6721ff3e39a6f9b55d87ec2b72bc5bb55ea806ba75aa6c27b2a335df",
        "b7ccf75f73f871b1d67fdba1aa2ad223fa1ce310ba9fa6eff0d7c10efca9104b",
        "c889bbe3b168749002cd45dc59f5c64caa69fdd83ea3220b921c4cbeb3e09323",
        "1eb435d6fe5be7049884035e84c4ce9a827c0f873dfa379856fff6d5ff2590b4",
        "450fc3fd607df08aa38e8e76e6bcb6fd3b175b7fb9ce15e801bfe3aa59fff47f",
        "7e7d5116103f450e9e61eb6cb7014072b400e97e91b145eb8121f7832555937c",
        "e9b956e1c67ab5f06f5a1f01f40524269e03eb29fbf1476640f9cdd219b8c629",
        null
    ]
}
```
