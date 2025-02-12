---
description: >-
  Example code for the /node/peer-list/all rest method. Сomplete guide on how to
  use /node/peer-list/all rest in GetBlock.io Web3 documentation.
---

# /node/peer-list/all - NEM

#### Parameters

\-

#### Request

```java
curl --location --request GET 'https://xem.getblock.io/node/peer-list/all' 
 --header 'x-api-key: YOUR-API-KEY' 
 --header 'Content-Type: application/json'
```

#### Response

```java
{
    "active": [
        {
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
        {
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
        }
    ],
    "busy": [],
    "failure": [],
    "inactive": [
        {
            "endpoint": {
                "host": "209.126.98.204",
                "port": 7890,
                "protocol": "http"
            },
            "identity": {
                "name": "alice4",
                "public-key": "d11ada95014685240ae69b0e44d60f931e61d444604a6a1b74ea085360dca396"
            },
            "metaData": {
                "application": null,
                "features": 0,
                "networkId": 0,
                "platform": null,
                "version": "0.0.0"
            }
        },
        {
            "endpoint": {
                "host": "62.75.251.134",
                "port": 7890,
                "protocol": "http"
            },
            "identity": {
                "name": "alice2",
                "public-key": "cd94cdcfde6878e093bc70e35b575dbe68095c69f73112e67559f71c1fb64c6e"
            },
            "metaData": {
                "application": null,
                "features": 0,
                "networkId": 0,
                "platform": null,
                "version": "0.0.0"
            }
        }
    ]
}
```
