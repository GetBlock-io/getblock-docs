---
description: >-
  Example code for the fundrawtransaction json-rpc method. Сomplete guide on how
  to use fundrawtransaction json-rpc in GetBlock.io Web3 documentation.
---

# fundrawtransaction - Bitcoin

#### Parameters

`hexstring` - string, required

The hex string of the raw transaction

`options` - json object, optional

for backward compatibility: passing in a true instead of an object will result in {"includeWatching":true}

`iswitness` - boolean, optional, default=depends on heuristic tests

Whether the transaction hex is a serialized witness transaction.

If iswitness is not present, heuristic tests will be used in decoding.

If true, only witness deserialization will be tried.

If false, only non-witness deserialization will be tried.

This boolean should reflect whether the transaction has inputs (e.g. fully valid, or on-chain transactions), if known by the caller.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "fundrawtransaction",
"params": ["hexstring", "", null],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": "01000000000101e17e03d21d051aa2bd9d336c3ac0693cfa92ce71592ceec521b1c48019ff77a101000000171600146d76e574b5f4825fe740ba6c41aaf1b319dfb80cffffffff02819a010000000000160014422002d927a1cae901eac668444cce8dd0ae60d529b31b0b0000000017a914f5b48d1130dc3d366d1eabf6783a552d1c8e08f4870247304402206701306a4750908fd48dead54331a3c7b4dce04ec10bfc6dd32049e2cff061a5022013c9d66827fabbeaadeb30b41c09aca2daddf4628cd00e3b993b1c86a12ff5190121034bcb9be1daf6ce1193774d15f863768b621bc95a363f1da5810129e961a2317400000000"
}
```
