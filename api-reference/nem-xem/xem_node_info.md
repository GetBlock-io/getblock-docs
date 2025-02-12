---
description: >-
  Example code for the /node/info rest method. Сomplete guide on how to use
  /node/info rest in GetBlock.io Web3 documentation.
---

# /node/info - NEM

#### Parameters

\-

#### Request

```java
curl --location --request GET 'https://xem.getblock.io/node/info' 
 --header 'x-api-key: YOUR-API-KEY' 
 --header 'Content-Type: application/json'
```

#### Response

```java
{
    "endpoint": {
        "host": "31.172.74.35",
        "port": 7890,
        "protocol": "http"
    },
    "identity": {
        "name": "NDX7DFK43JV6HT2ZCKF5D37JBZJJA6KMFURXDVKL",
        "public-key": "de23c19f3d8f55cb3d97ee5fc6c8131e0d348bfc5ee1e31d775bdc0a873eeb2e"
    },
    "metaData": {
        "application": null,
        "features": 1,
        "networkId": 104,
        "platform": "Private Build (1.8.0_292) on Linux",
        "version": "0.6.100"
    }
}
```
