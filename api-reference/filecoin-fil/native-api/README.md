---
description: >-
  JSON-RPC API reference for the Filecoin native (Lotus) API. Explore method
  list, request examples, and how to connect to GetBlock's Filecoin RPC
  endpoints
---

# Native API

The Filecoin native API is the Lotus JSON-RPC interface. Methods use `Filecoin.<Method>` names and Filecoin data structures: CIDs (as `{"/": "bafy..."}`), tipsets, actors, and attoFIL balances (1 FIL = 10^18 attoFIL). Read methods require no authentication token.

## Base URL

```bash
https://shared.eu-central-1.getblock.io/
```

Send a `POST` request with a JSON-RPC 2.0 body. Many state methods take a **tipset key** as their final parameter — an array of block CIDs identifying the tipset to read against. Pass an empty array `[]` to read the current chain head.

## Chain

| Method                          | Description                   |
| ------------------------------- | ----------------------------- |
| Filecoin.ChainHead              | The current chain head tipset |
| Filecoin.ChainGetTipSetByHeight | The tipset at an epoch        |
| Filecoin.ChainGetBlock          | A block header by CID         |
| Filecoin.ChainGetBlockMessages  | The messages in a block       |
| Filecoin.ChainGetMessage        | A message by CID              |

## State

| Method                       | Description                           |
| ---------------------------- | ------------------------------------- |
| Filecoin.StateGetActor       | An actor's nonce and balance          |
| Filecoin.StateLookupID       | The ID address for an address         |
| Filecoin.StateAccountKey     | The public-key address for an account |
| Filecoin.StateMinerPower     | A miner's storage power               |
| Filecoin.StateNetworkVersion | The network protocol version          |

## Wallet, Mpool & Gas

| Method                         | Description                   |
| ------------------------------ | ----------------------------- |
| Filecoin.WalletBalance         | An address balance in attoFIL |
| Filecoin.MpoolGetNonce         | The next nonce for a sender   |
| Filecoin.GasEstimateMessageGas | Fill a message's gas fields   |

## Node

| Method           | Description                  |
| ---------------- | ---------------------------- |
| Filecoin.Version | Node version and block delay |

## Support

* Support: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [Lotus JSON-RPC Reference](https://docs.filecoin.io/reference/json-rpc)
* [Filecoin (FIL)](../)
