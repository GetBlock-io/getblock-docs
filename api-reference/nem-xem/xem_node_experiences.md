---
description: >-
  Example code for the /node/experiences rest method. Сomplete guide on how to
  use /node/experiences rest in GetBlock.io Web3 documentation.
---

# /node/experiences - NEM

#### Parameters

\-

#### Request

```java
curl --location --request GET 'https://xem.getblock.io/node/experiences' 
 --header 'x-api-key: YOUR-API-KEY' 
 --header 'Content-Type: application/json'
```

#### Response

```java
{
    "data": [
        {
            "experience": {
                "f": 0,
                "s": 0
            },
            "node": {
                "endpoint": {
                    "host": "89.250.243.166",
                    "port": 7890,
                    "protocol": "http"
                },
                "identity": {
                    "name": "LinuxCZ2",
                    "public-key": "76f1787ea817a01b033d79725146c1c5063c7e943009bcd7e36b58051541ed9f"
                },
                "metaData": {
                    "application": null,
                    "features": 1,
                    "networkId": 104,
                    "platform": "Red Hat, Inc. (11.0.16) on Linux",
                    "version": "0.6.100"
                }
            },
            "syncs": 587
        },
        {
            "experience": {
                "f": 0,
                "s": 3
            },
            "node": {
                "endpoint": {
                    "host": "54.250.98.33",
                    "port": 7890,
                    "protocol": "http"
                },
                "identity": {
                    "name": "NC4T246ALCPNBTAOCSC5EAVFMDFBOACSQAF6WKHV",
                    "public-key": "1e77b69e83b1d07e8deaa564361f5a159e47a4e51c10352f71ce60937dff5caf"
                },
                "metaData": {
                    "application": null,
                    "features": 1,
                    "networkId": 104,
                    "platform": "Private Build (1.8.0_265) on Linux",
                    "version": "0.6.100"
                }
            },
            "syncs": 640
        }
    ]
}
```
