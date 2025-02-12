---
description: >-
  Example code for the gobject_getcurrentvotes json-rpc method. Сomplete guide
  on how to use gobject_getcurrentvotes json-rpc in GetBlock.io Web3
  documentation.
---

# gobject\_getcurrentvotes - Dash

#### Parameters

`method name` - string

None

`governance-hash` - string (hex)

The hash of a governance object.

#### Request

```java
curl --location --request POST 'https://dash.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "gobject",
"params": ["getcurrentvotes", "1f8ae9f6e435a22ce74139997bad70384a84452f95c5054a2c76602609ddc700"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": {
        "0071b0ca9b0849264dce527877756a73cbc967a092c0f30a5d9df611d1c28eb9": "1b29605dd8f60ef493454bb5204d4841f4abbfb4b5c2bc22b35a611363f33f90-1:1629812867:yes:funding",
        "007e8097fc0dc9e2dc42e9f6d86abdef757208720986dc88a3449cd6237f2cc4": "1f75be94d04094611e84eebac0093635202a26f8241426e4cb46cc428138c7f0-1:1629683994:yes:funding",
        "032bd71bdcaa4b215fce628b0264cffb3ff0cc294a8e440d1c30b09255ea42f1": "982d22c86ce9b515d1ad1630f8ed8a9170557ded3370332b42076626068aae81-0:1627834267:abstain:funding",
        "0338c49397e42d36a696f1312325d967031f3e53721a7a3e52d13de80a7f9813": "dc3960df006cb96b616e7690812c601d1e3357899cee8e3bdfa738c9d602e87d-1:1629669677:yes:funding",
        "03824fff16a90907f3c6e82335f4aa06fc5438acea3643a1d3707dffc383687c": "ef8319a1187bd7b7b37d04f6df7c444c949dabc84ddf8f9148bc22c3abf0b834-1:1629092331:abstain:funding",
        "0382ae7c6cefff6d7220dce350870c0b8f2f6a907bf799f2c592ca27e7e21f54": "09bee6942eb5e00f43c9fcf9bfaa8ac05e951447908f5b989e54a3dc87aa16f9-0:1629682561:yes:funding",
        "03e31b5bf7a63927d7545dcf95810f8ac6149814395a8745757472234b578f55": "b7a57f160951ccda4c475d0ecf4f1b3899325c493b23a7634a2b31f957ce22d4-1:1628012799:yes:funding",
        "0484fbabc00431bd9efc01ff9f5796cfc80ec69d38d1d9934c405e3b736b0146": "882d6a3a24c8f56ad9e2ff03d1c467d3902189f976dc1a0c2a26dc3c217c7b6c-1:1629624407:no:funding",
        "05241968de09deaa259c00ac6369784f2dc17b2892ad1901597296a7d3e457d5": "56f9475cf2687a382ac613f3d055c37069819fa3899f40ff76a8784735e36af5-1:1627851312:yes:funding",
        "055b2ca00c7de5969ce59a2073d584890dd19037c445917f05d039d86bcc6f72": "c6ee7d66db1a740ba3f297ead98c2ea16e8b75c2cce1322952aa96b5ba974463-3:1630440928:no:funding",
        "05ae983b15922641d863cefa0dd3d1ae1763fe4825726a611dba8ac0104177a3": "e18a948ed5e29d9e5a27633c9fa2feb865c875f703164cbb65e55ed0ef4fe6cc-1:1629831738:no:funding",
        "05d5e2985973545dc4c1e9e71ef6990ee12a8b5674c571c27313ef83a13672e7": "725f32a0f99ac925764f2a056d555ce902643c941976ac73d6e5347d16fde392-1:1629830946:no:funding"
    }
}
```
