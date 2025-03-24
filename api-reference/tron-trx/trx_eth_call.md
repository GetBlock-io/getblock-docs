---
description: >-
  Example code for the eth_call json-rpc method. Сomplete guide on how to use
  eth_call json-rpc in GetBlock.io Web3 documentation.
---

# eth\_call - TRON

#### Parameters

`object` - object

The transaction call object, the items in it as below: - from - DATA, 20 bytes - Caller address - to - DATA, 20 bytes - Contract address - gas - QUANTITY - Not supported. The value is 0x0 - gasPrice - QUANTITY - Not supported. The value is 0x0 - value - QUANTITY - Not supported. The value is 0x0 - data - DATA - Hash of the method signature and encoded parameters.

`QUANTITY|TAG` - string

currently, only "latest" is available.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "eth_call",
"params": [{"from": "0x41F0CC5A2A84CD0F68ED1667070934542D673ACBD8", "to": "0x4170082243784DCDF3042034E7B044D6D342A91360", "gas": "0x01", "gasPrice": "0x8c", "value": "0x01", "data": "0x70a08231000000000000000000000041f0cc5a2a84cd0f68ed1667070934542d673acbd8"}, "latest"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "0x"
}
```
