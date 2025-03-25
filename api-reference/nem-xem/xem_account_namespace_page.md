---
description: >-
  Example code for the /account/namespace/page rest method. Сomplete guide on
  how to use /account/namespace/page rest in GetBlock.io Web3 documentation.
---

# /account/namespace/page - NEM

#### Parameters

`address` -

The address of the account.

`parent` -

The optional parent namespace id.

`id` -

The optional namespace database id up to which namespaces are returned.

`pageSize` -

The (optional) number of namespaces to be returned.

#### Request

```java
curl --location --request GET 'https://go.getblock.io/<ACCESS-TOKEN>/account/namespace/page?address=NC4T246ALCPNBTAOCSC5EAVFMDFBOACSQAF6WKHV&pageSize=5' \
--header 'Content-Type: application/json'
```

#### Response

```java
{
    "data": [
        {
            "fqn": "makoto.metal.coins",
            "owner": "TD3RXTHBLK6J3UD2BH2PXSOFLPWZOTR34WCG4HXH",
            "height": 13465
        }
    ]
}
```
