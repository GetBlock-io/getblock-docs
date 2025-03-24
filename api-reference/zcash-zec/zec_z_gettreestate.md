---
description: >-
  Example code for the z_gettreestate json-rpc method. Сomplete guide on how to
  use z_gettreestate json-rpc in GetBlock.io Web3 documentation.
---

# z\_gettreestate - Zcash

#### Parameters

`hash|height` - string

The block hash or height. Height can be negative where -1 is the last known valid block

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "z_gettreestate",
"params": ["1384123"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": {
        "hash": "0000000000938f9fc631767d1302cd7a2cc29fce45f82724eb4d55d11658768f",
        "height": 1384123,
        "sapling": {
            "commitments": {
                "finalRoot": "5c14d2248db227e56eaa60f24aca1167bf6361aec8b998eb9beb10cfd7b47b69",
                "finalState": "01583b6d5516b031d2ff6ec4edd7af3a83f462476c4e198653f7475d47e2d1367200130001176603e470d4053871c4456c7d8e095174395cdb7de604f5f91af78ee4afc92d0145ce31f08387fae4cb9ece952f6419c0d96870d67a42154ac59bc7aa70d81b6a000001163b13d26525f4db46b8cae399713100410a5e2f504407add8a6c24e8f3cbd00012c5f2a90d22bc36f430a33fad6fff96ff4ea3c985c13c4e7da9f401c1314ba49016db986e3b590002dffaaedc4bb1076a719235d728557c83ffcd1c35867beea000171359d67c3401002cf7c5d4a6262eddffca68156446b72009441b61629ac7e5b000001966669f15cd0722a22aea47bc00de94afbbdfbaf5f07eb9e8909aeea44dd8633012bcf3a07cce1cd7126e5d70dbd8d1a1eb88bc0bf9733b0ef311472245137b265000001cdd92a1e884cf1914dae8345423203832ec7bbf9d95ae50b82e4327b39d6d9120125911f4524469c00ccb1ba69e64f0ee7380c8d17bbfc76ecd238421b86eb6e09000118f64df255c9c43db708255e7bf6bffd481e5c2f38fe9ed8f3d189f7f9cf2644"
            }
        },
        "sprout": {
            "commitments": {
                "finalRoot": "318bcf0c2fad68a10606adba4704c457279ad68a6c82138721158a6ed73680a8",
                "finalState": "0185ffe97e4e9f4e1177eca799b1cb71d442eadb0dc263f9a7aba3fa1dc9906a7501ed777281cf45c3ce588e9cdbf0b9d7fc62f57c491674d057769929d2086de528140001cde2b1f177c035039c743903eedbede86acc664b5c7a7a3a212b4a5705827a0a018e2e305b408488986cee294ad108978fe0100578afb45c0b7e8abe6e8fd7202c01cd02d11674a7fc848d7bf7e1f5294bfe169fa2e8b10396ec77bb66a1f73b91cf01b90243b2eab542be76b12f67521457701c5e57e9d9fd1c3d539933ae47b2a5f8012b00da638cb9e87b52c79d264b71e7ae5d2f51f37a02dbe6a6942d1744e7bb0600000000000196295a1e8d018c1c59f098e69350c7a206b850c828710fbaeb86d3ef72da3bb6000001806212ff57170f6904d462d24bc5df6112269a0b65defffbd4b78ff9a7859b9d0183a03915c566ab12ba59b130bab08a25b932ce204bf1774fc9e9267e5130f19101465dbb7f98de7f19e22f37c537660e9447fc27e1839238dc75548b047c41d1be01c14d7b1dfae9c25c284218d74c43621b795246a5fb3c238c2ca9d7f91d5aa86000011b1fc3241793d87854115ec7190ea06c64b76faa971c77ccc6316ec716f90f77"
            }
        },
        "time": 1631173584
    }
}
```
