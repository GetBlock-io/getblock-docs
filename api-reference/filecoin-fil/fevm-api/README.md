# FEVM API

The Filecoin EVM (FEVM) is an EVM-compatible runtime on Filecoin. It exposes the standard Ethereum JSON-RPC method set through the `eth_` namespace, so Solidity contracts and Ethereum tooling — Hardhat, Foundry, ethers.js, viem, and web3.py — work on Filecoin. The FEVM chain ID is `314` (Calibration testnet is `314159`), and the native token is FIL (base unit attoFIL).

Addresses in the FEVM layer use the standard `0x` form. A Filecoin `f4` address and its `0x` form map to the same account, so contracts and accounts are reachable from both the native and FEVM surfaces.

## Base URL

```bash
https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
```

Send a `POST` request with a JSON-RPC 2.0 body. Replace `<ACCESS-TOKEN>` with the access token from the GetBlock dashboard.

## Account & State

| Method                   | Description              |
| ------------------------ | ------------------------ |
| eth\_getBalance          | eth\_getBalance          |
| eth\_getTransactionCount | eth\_getTransactionCount |
| eth\_getCode             | eth\_getCode             |
| eth\_getStorageAt        | eth\_getStorageAt        |

## Execution

| Method           | Description      |
| ---------------- | ---------------- |
| eth\_call        | eth\_call        |
| eth\_estimateGas | eth\_estimateGas |

## Gas & Fees

| Method                    | Description               |
| ------------------------- | ------------------------- |
| eth\_gasPrice             | eth\_gasPrice             |
| eth\_maxPriorityFeePerGas | eth\_maxPriorityFeePerGas |
| eth\_feeHistory           | eth\_feeHistory           |

## Blocks

| Method                                | Description                           |
| ------------------------------------- | ------------------------------------- |
| eth\_blockNumber                      | eth\_blockNumber                      |
| eth\_getBlockByNumber                 | eth\_getBlockByNumber                 |
| eth\_getBlockByHash                   | eth\_getBlockByHash                   |
| eth\_getBlockTransactionCountByNumber | eth\_getBlockTransactionCountByNumber |
| eth\_getBlockTransactionCountByHash   | eth\_getBlockTransactionCountByHash   |
| eth\_getBlockReceipts                 | eth\_getBlockReceipts                 |

## Transactions

| Method                                   | Description                              |
| ---------------------------------------- | ---------------------------------------- |
| eth\_getTransactionByHash                | eth\_getTransactionByHash                |
| eth\_getTransactionReceipt               | eth\_getTransactionReceipt               |
| eth\_getTransactionByBlockNumberAndIndex | eth\_getTransactionByBlockNumberAndIndex |
| eth\_getTransactionByBlockHashAndIndex   | eth\_getTransactionByBlockHashAndIndex   |
| eth\_sendRawTransaction                  | eth\_sendRawTransaction                  |

## Logs

| Method       | Description  |
| ------------ | ------------ |
| eth\_getLogs | eth\_getLogs |

## Node & Network

| Method              | Description         |
| ------------------- | ------------------- |
| eth\_chainId        | eth\_chainId        |
| net\_version        | net\_version        |
| web3\_clientVersion | web3\_clientVersion |
| eth\_syncing        | eth\_syncing        |

## Debug

| Method                    | Description               |
| ------------------------- | ------------------------- |
| debug\_traceTransaction   | debug\_traceTransaction   |
| debug\_traceBlockByNumber | debug\_traceBlockByNumber |
| debug\_traceBlockByHash   | debug\_traceBlockByHash   |

## Support

* Support: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [Ethereum JSON-RPC Specification](https://ethereum.org/en/developers/docs/apis/json-rpc/)
* [Filecoin (FIL)](../)
