---
description: >-
  Example code for the /debug/connections/timers rest method. Сomplete guide on
  how to use /debug/connections/timers rest in GetBlock.io Web3 documentation.
---

# /debug/connections/timers - NEM

#### Parameters

\-

#### Request

```java
curl --location --request GET 'https://go.getblock.io/YOUR-ACCESS-TOKEN/debug/connections/timers' \
--header 'Content-Type: application/json'
```

#### Response

```java
{
    "data": [
        {
            "last-delay-time": 3000,
            "executions": 1024,
            "failures": 0,
            "successes": 1024,
            "last-operation-start-time": 9317695,
            "is-executing": 0,
            "name": "FORAGING",
            "average-operation-time": 0,
            "last-operation-time": 0
        },
        {
            "last-delay-time": 74181,
            "executions": 71,
            "failures": 0,
            "successes": 71,
            "last-operation-start-time": 9317654,
            "is-executing": 0,
            "name": "REFRESH",
            "average-operation-time": 6,
            "last-operation-time": 7
        }
    ]
}
```
