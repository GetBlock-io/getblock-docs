---
description: >-
  Example code for the eth_getTransactionReceipt json-rpc method. Сomplete guide
  on how to use eth_getTransactionReceipt json-rpc in GetBlock.io Web3
  documentation.
---

# eth\_getTransactionReceipt - Huobi ECO Chain

#### Parameters

`DATA` - string

Hash of a transaction.

#### Request

```java
curl --location --request POST 'https://heco.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getTransactionReceipt",
"params": ["0xf32e8f9bbaaac3cc64cfd8547033435a56e830165c53369d9af9a205101f503b"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "blockHash": "0x779f94a2fad68383ed932e9f8fd04234a92b85f9d849bec2188c675dcdb0f16d",
        "blockNumber": "0x62ccd5",
        "contractAddress": null,
        "cumulativeGasUsed": "0x6bcc9",
        "effectiveGasPrice": "0xd0ad054b",
        "from": "0x9a0894cca9fe2c619bf58ff633d7e2c0c05f96de",
        "gasUsed": "0x6bcc9",
        "logs": [
            {
                "address": "0xa71edc38d189767582c38a3145b5873052c3e47a",
                "blockHash": "0x779f94a2fad68383ed932e9f8fd04234a92b85f9d849bec2188c675dcdb0f16d",
                "blockNumber": "0x62ccd5",
                "data": "0x00000000000000000000000000000000000000000000000029c5ab0d65ecff2a",
                "logIndex": "0x0",
                "removed": false,
                "topics": [
                    "0x8c5be1e5ebec7d5bd14f71427d1e84f3dd0314c0f7b2291e5b200ac8c7c3b925",
                    "0x000000000000000000000000a7df0f4affe5cc11df55c59e56fde237f4b42371",
                    "0x000000000000000000000000ed7d5f38c79115ca12fe6c0041abb22f0a06c300"
                ],
                "transactionHash": "0xf32e8f9bbaaac3cc64cfd8547033435a56e830165c53369d9af9a205101f503b",
                "transactionIndex": "0x0"
            },
            {
                "address": "0xa71edc38d189767582c38a3145b5873052c3e47a",
                "blockHash": "0x779f94a2fad68383ed932e9f8fd04234a92b85f9d849bec2188c675dcdb0f16d",
                "blockNumber": "0x62ccd5",
                "data": "0x00000000000000000000000000000000000000000000000029c5ab0d65ecff2a",
                "logIndex": "0x1",
                "removed": false,
                "topics": [
                    "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                    "0x000000000000000000000000a7df0f4affe5cc11df55c59e56fde237f4b42371",
                    "0x0000000000000000000000000902c53024a1dae8d15b2eb7633ea331c732c03e"
                ],
                "transactionHash": "0xf32e8f9bbaaac3cc64cfd8547033435a56e830165c53369d9af9a205101f503b",
                "transactionIndex": "0x0"
            },
            {
                "address": "0xa71edc38d189767582c38a3145b5873052c3e47a",
                "blockHash": "0x779f94a2fad68383ed932e9f8fd04234a92b85f9d849bec2188c675dcdb0f16d",
                "blockNumber": "0x62ccd5",
                "data": "0x0000000000000000000000000000000000000000000000000000000000000000",
                "logIndex": "0x2",
                "removed": false,
                "topics": [
                    "0x8c5be1e5ebec7d5bd14f71427d1e84f3dd0314c0f7b2291e5b200ac8c7c3b925",
                    "0x000000000000000000000000a7df0f4affe5cc11df55c59e56fde237f4b42371",
                    "0x000000000000000000000000ed7d5f38c79115ca12fe6c0041abb22f0a06c300"
                ],
                "transactionHash": "0xf32e8f9bbaaac3cc64cfd8547033435a56e830165c53369d9af9a205101f503b",
                "transactionIndex": "0x0"
            },
            {
                "address": "0x180dae91d6d56235453a892d2e56a3e40ba81df8",
                "blockHash": "0x779f94a2fad68383ed932e9f8fd04234a92b85f9d849bec2188c675dcdb0f16d",
                "blockNumber": "0x62ccd5",
                "data": "0x0000000000000000000000000000000000000000003b44db7a29f9544097a126",
                "logIndex": "0x3",
                "removed": false,
                "topics": [
                    "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                    "0x0000000000000000000000000902c53024a1dae8d15b2eb7633ea331c732c03e",
                    "0x0000000000000000000000003845bcd555bbe6aeedd44281ec05b8cdf9cda106"
                ],
                "transactionHash": "0xf32e8f9bbaaac3cc64cfd8547033435a56e830165c53369d9af9a205101f503b",
                "transactionIndex": "0x0"
            },
            {
                "address": "0x0902c53024a1dae8d15b2eb7633ea331c732c03e",
                "blockHash": "0x779f94a2fad68383ed932e9f8fd04234a92b85f9d849bec2188c675dcdb0f16d",
                "blockNumber": "0x62ccd5",
                "data": "0x00000000000000000000000000000000000000350e9a080872b9cae2e4cda2f800000000000000000000000000000000000000000000254851471c240f241602",
                "logIndex": "0x4",
                "removed": false,
                "topics": [
                    "0x1c411e9a96e071241c2f21f7726b17ae89e3cab4c78be50e062b03a9fffbbad1"
                ],
                "transactionHash": "0xf32e8f9bbaaac3cc64cfd8547033435a56e830165c53369d9af9a205101f503b",
                "transactionIndex": "0x0"
            },
            {
                "address": "0x0902c53024a1dae8d15b2eb7633ea331c732c03e",
                "blockHash": "0x779f94a2fad68383ed932e9f8fd04234a92b85f9d849bec2188c675dcdb0f16d",
                "blockNumber": "0x62ccd5",
                "data": "0x000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000029c5ab0d65ecff2a0000000000000000000000000000000000000000003b44db7a29f9544097a1260000000000000000000000000000000000000000000000000000000000000000",
                "logIndex": "0x5",
                "removed": false,
                "topics": [
                    "0xd78ad95fa46c994b6551d0da85fc275fe613ce37657fb8d5e3d130840159d822",
                    "0x000000000000000000000000ed7d5f38c79115ca12fe6c0041abb22f0a06c300",
                    "0x0000000000000000000000003845bcd555bbe6aeedd44281ec05b8cdf9cda106"
                ],
                "transactionHash": "0xf32e8f9bbaaac3cc64cfd8547033435a56e830165c53369d9af9a205101f503b",
                "transactionIndex": "0x0"
            },
            {
                "address": "0x25d2e80cb6b86881fd7e07dd263fb79f4abe033c",
                "blockHash": "0x779f94a2fad68383ed932e9f8fd04234a92b85f9d849bec2188c675dcdb0f16d",
                "blockNumber": "0x62ccd5",
                "data": "0x0000000000000000000000000000000000000000000000001bd7ef5e38d1279d",
                "logIndex": "0x6",
                "removed": false,
                "topics": [
                    "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                    "0x0000000000000000000000003845bcd555bbe6aeedd44281ec05b8cdf9cda106",
                    "0x000000000000000000000000fc021c1ec170c7b320cfd4de99e83a91e7eaabbc"
                ],
                "transactionHash": "0xf32e8f9bbaaac3cc64cfd8547033435a56e830165c53369d9af9a205101f503b",
                "transactionIndex": "0x0"
            },
            {
                "address": "0x3845bcd555bbe6aeedd44281ec05b8cdf9cda106",
                "blockHash": "0x779f94a2fad68383ed932e9f8fd04234a92b85f9d849bec2188c675dcdb0f16d",
                "blockNumber": "0x62ccd5",
                "data": "0x0000000000000000000000000000000000000000166f8749c11a0abf400df4b100000000000000000000000000000000000000000000000a7665dcd5ee11b0a1",
                "logIndex": "0x7",
                "removed": false,
                "topics": [
                    "0x1c411e9a96e071241c2f21f7726b17ae89e3cab4c78be50e062b03a9fffbbad1"
                ],
                "transactionHash": "0xf32e8f9bbaaac3cc64cfd8547033435a56e830165c53369d9af9a205101f503b",
                "transactionIndex": "0x0"
            },
            {
                "address": "0x3845bcd555bbe6aeedd44281ec05b8cdf9cda106",
                "blockHash": "0x779f94a2fad68383ed932e9f8fd04234a92b85f9d849bec2188c675dcdb0f16d",
                "blockNumber": "0x62ccd5",
                "data": "0x0000000000000000000000000000000000000000003b44db7a29f9544097a126000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000001bd7ef5e38d1279d",
                "logIndex": "0x8",
                "removed": false,
                "topics": [
                    "0xd78ad95fa46c994b6551d0da85fc275fe613ce37657fb8d5e3d130840159d822",
                    "0x000000000000000000000000ed7d5f38c79115ca12fe6c0041abb22f0a06c300",
                    "0x000000000000000000000000fc021c1ec170c7b320cfd4de99e83a91e7eaabbc"
                ],
                "transactionHash": "0xf32e8f9bbaaac3cc64cfd8547033435a56e830165c53369d9af9a205101f503b",
                "transactionIndex": "0x0"
            },
            {
                "address": "0xae3a768f9ab104c69a7cd6041fe16ffa235d1810",
                "blockHash": "0x779f94a2fad68383ed932e9f8fd04234a92b85f9d849bec2188c675dcdb0f16d",
                "blockNumber": "0x62ccd5",
                "data": "0x00000000000000000000000000000000000000000000000000d93532e5511e15",
                "logIndex": "0x9",
                "removed": false,
                "topics": [
                    "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                    "0x000000000000000000000000fc021c1ec170c7b320cfd4de99e83a91e7eaabbc",
                    "0x000000000000000000000000a7df0f4affe5cc11df55c59e56fde237f4b42371"
                ],
                "transactionHash": "0xf32e8f9bbaaac3cc64cfd8547033435a56e830165c53369d9af9a205101f503b",
                "transactionIndex": "0x0"
            },
            {
                "address": "0xfc021c1ec170c7b320cfd4de99e83a91e7eaabbc",
                "blockHash": "0x779f94a2fad68383ed932e9f8fd04234a92b85f9d849bec2188c675dcdb0f16d",
                "blockNumber": "0x62ccd5",
                "data": "0x0000000000000000000000000000000000000000000015651e8fbda924b85b8f0000000000000000000000000000000000000000000000a766dc63b716b4f589",
                "logIndex": "0xa",
                "removed": false,
                "topics": [
                    "0x1c411e9a96e071241c2f21f7726b17ae89e3cab4c78be50e062b03a9fffbbad1"
                ],
                "transactionHash": "0xf32e8f9bbaaac3cc64cfd8547033435a56e830165c53369d9af9a205101f503b",
                "transactionIndex": "0x0"
            },
            {
                "address": "0xfc021c1ec170c7b320cfd4de99e83a91e7eaabbc",
                "blockHash": "0x779f94a2fad68383ed932e9f8fd04234a92b85f9d849bec2188c675dcdb0f16d",
                "blockNumber": "0x62ccd5",
                "data": "0x0000000000000000000000000000000000000000000000001bd7ef5e38d1279d0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000d93532e5511e15",
                "logIndex": "0xb",
                "removed": false,
                "topics": [
                    "0xd78ad95fa46c994b6551d0da85fc275fe613ce37657fb8d5e3d130840159d822",
                    "0x000000000000000000000000ed7d5f38c79115ca12fe6c0041abb22f0a06c300",
                    "0x000000000000000000000000a7df0f4affe5cc11df55c59e56fde237f4b42371"
                ],
                "transactionHash": "0xf32e8f9bbaaac3cc64cfd8547033435a56e830165c53369d9af9a205101f503b",
                "transactionIndex": "0x0"
            },
            {
                "address": "0xae3a768f9ab104c69a7cd6041fe16ffa235d1810",
                "blockHash": "0x779f94a2fad68383ed932e9f8fd04234a92b85f9d849bec2188c675dcdb0f16d",
                "blockNumber": "0x62ccd5",
                "data": "0x00000000000000000000000000000000000000000000000000d93532e5511e15",
                "logIndex": "0xc",
                "removed": false,
                "topics": [
                    "0x8c5be1e5ebec7d5bd14f71427d1e84f3dd0314c0f7b2291e5b200ac8c7c3b925",
                    "0x000000000000000000000000a7df0f4affe5cc11df55c59e56fde237f4b42371",
                    "0x000000000000000000000000e38623b265b5acc9f35e696381769e556ed932f9"
                ],
                "transactionHash": "0xf32e8f9bbaaac3cc64cfd8547033435a56e830165c53369d9af9a205101f503b",
                "transactionIndex": "0x0"
            },
            {
                "address": "0xae3a768f9ab104c69a7cd6041fe16ffa235d1810",
                "blockHash": "0x779f94a2fad68383ed932e9f8fd04234a92b85f9d849bec2188c675dcdb0f16d",
                "blockNumber": "0x62ccd5",
                "data": "0x00000000000000000000000000000000000000000000000000d93532e5511e15",
                "logIndex": "0xd",
                "removed": false,
                "topics": [
                    "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                    "0x000000000000000000000000a7df0f4affe5cc11df55c59e56fde237f4b42371",
                    "0x000000000000000000000000b44ff0a3a789c5a4e6b6f8eac58227b24a6b4448"
                ],
                "transactionHash": "0xf32e8f9bbaaac3cc64cfd8547033435a56e830165c53369d9af9a205101f503b",
                "transactionIndex": "0x0"
            },
            {
                "address": "0xae3a768f9ab104c69a7cd6041fe16ffa235d1810",
                "blockHash": "0x779f94a2fad68383ed932e9f8fd04234a92b85f9d849bec2188c675dcdb0f16d",
                "blockNumber": "0x62ccd5",
                "data": "0x0000000000000000000000000000000000000000000000000000000000000000",
                "logIndex": "0xe",
                "removed": false,
                "topics": [
                    "0x8c5be1e5ebec7d5bd14f71427d1e84f3dd0314c0f7b2291e5b200ac8c7c3b925",
                    "0x000000000000000000000000a7df0f4affe5cc11df55c59e56fde237f4b42371",
                    "0x000000000000000000000000e38623b265b5acc9f35e696381769e556ed932f9"
                ],
                "transactionHash": "0xf32e8f9bbaaac3cc64cfd8547033435a56e830165c53369d9af9a205101f503b",
                "transactionIndex": "0x0"
            },
            {
                "address": "0xa71edc38d189767582c38a3145b5873052c3e47a",
                "blockHash": "0x779f94a2fad68383ed932e9f8fd04234a92b85f9d849bec2188c675dcdb0f16d",
                "blockNumber": "0x62ccd5",
                "data": "0x0000000000000000000000000000000000000000000000002a0a808c88aa1572",
                "logIndex": "0xf",
                "removed": false,
                "topics": [
                    "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                    "0x000000000000000000000000b44ff0a3a789c5a4e6b6f8eac58227b24a6b4448",
                    "0x000000000000000000000000a7df0f4affe5cc11df55c59e56fde237f4b42371"
                ],
                "transactionHash": "0xf32e8f9bbaaac3cc64cfd8547033435a56e830165c53369d9af9a205101f503b",
                "transactionIndex": "0x0"
            },
            {
                "address": "0xb44ff0a3a789c5a4e6b6f8eac58227b24a6b4448",
                "blockHash": "0x779f94a2fad68383ed932e9f8fd04234a92b85f9d849bec2188c675dcdb0f16d",
                "blockNumber": "0x62ccd5",
                "data": "0x0000000000000000000000000000000000000000000000e6d9f122dec8fe3c31000000000000000000000000000000000000000000000004a5fabda9e2586170",
                "logIndex": "0x10",
                "removed": false,
                "topics": [
                    "0x1c411e9a96e071241c2f21f7726b17ae89e3cab4c78be50e062b03a9fffbbad1"
                ],
                "transactionHash": "0xf32e8f9bbaaac3cc64cfd8547033435a56e830165c53369d9af9a205101f503b",
                "transactionIndex": "0x0"
            },
            {
                "address": "0xb44ff0a3a789c5a4e6b6f8eac58227b24a6b4448",
                "blockHash": "0x779f94a2fad68383ed932e9f8fd04234a92b85f9d849bec2188c675dcdb0f16d",
                "blockNumber": "0x62ccd5",
                "data": "0x000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000d93532e5511e150000000000000000000000000000000000000000000000002a0a808c88aa15720000000000000000000000000000000000000000000000000000000000000000",
                "logIndex": "0x11",
                "removed": false,
                "topics": [
                    "0xd78ad95fa46c994b6551d0da85fc275fe613ce37657fb8d5e3d130840159d822",
                    "0x000000000000000000000000e38623b265b5acc9f35e696381769e556ed932f9",
                    "0x000000000000000000000000a7df0f4affe5cc11df55c59e56fde237f4b42371"
                ],
                "transactionHash": "0xf32e8f9bbaaac3cc64cfd8547033435a56e830165c53369d9af9a205101f503b",
                "transactionIndex": "0x0"
            },
            {
                "address": "0x00000000687f5b66638856396bee28c1db0178d1",
                "blockHash": "0x779f94a2fad68383ed932e9f8fd04234a92b85f9d849bec2188c675dcdb0f16d",
                "blockNumber": "0x62ccd5",
                "data": "0x0000000000000000000000000000000000000000000000000000000000000000",
                "logIndex": "0x12",
                "removed": false,
                "topics": [
                    "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                    "0x000000000000000000000000a7df0f4affe5cc11df55c59e56fde237f4b42371",
                    "0x0000000000000000000000000000000000000000000000000000000000000000"
                ],
                "transactionHash": "0xf32e8f9bbaaac3cc64cfd8547033435a56e830165c53369d9af9a205101f503b",
                "transactionIndex": "0x0"
            }
        ],
        "logsBloom": "0x00200000000000040100000080002001000000000010800100000000000000000000500000000000020000000000002000000080040000008000000000200000000000002000080000000008000000200000000488000000000000000000000000000000020000000000020000000800000000000000000008000030081000000000202000000100000000000000000000000000000280080000004004000020020000000000000000000000408000000000000000800000000010000000000020000002100000430000000000000000005000000000001080000000000020000010000020000000000000800000000000000000008000000000008000200000",
        "status": "0x1",
        "to": "0xa7df0f4affe5cc11df55c59e56fde237f4b42371",
        "transactionHash": "0xf32e8f9bbaaac3cc64cfd8547033435a56e830165c53369d9af9a205101f503b",
        "transactionIndex": "0x0",
        "type": "0x0"
    }
}
```
