# Ogmios Websocket API

The Ogmios WebSocket interface carries the same JSON-RPC 2.0 protocol as the HTTP interface, over a persistent connection. It is required for the stateful Ouroboros mini-protocols, which hold session state across messages: chain synchronization, ledger-state acquisition, and mempool monitoring. The stateless queries documented under the JSON-RPC API also work over this connection.

## Base URL

```
wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
```

Replace `<ACCESS-TOKEN>` with the access token from the GetBlock dashboard. Open a WebSocket to this URL and exchange JSON-RPC 2.0 messages.

## Chain Synchronization

Find a starting point on the node's chain, then stream forward blocks and backward rollbacks.

| Method                                                                     | Description                         |
| -------------------------------------------------------------------------- | ----------------------------------- |
| [findIntersection](/broken/pages/9117be51cc97f83c4af6c9119956b06d541baac6) | Find a chain intersection point     |
| [nextBlock](/broken/pages/4d3497fbe06a05704601a91f048f5b8a06ee544d)        | Stream the next block or a rollback |

## Ledger-State Acquisition

Pin a fixed ledger point so a batch of state queries is answered consistently.

| Method             | Description                       |
| ------------------ | --------------------------------- |
| acquireLedgerState | Acquire a fixed ledger state      |
| releaseLedgerState | Release the acquired ledger state |

## Mempool Monitoring

Acquire a mempool snapshot, then list and inspect pending transactions.

| Method          | Description                              |
| --------------- | ---------------------------------------- |
| acquireMempool  | Acquire a mempool snapshot               |
| nextTransaction | Read the next mempool transaction        |
| hasTransaction  | Check if a transaction is in the mempool |
| sizeOfMempool   | Read mempool size and capacity           |
| releaseMempool  | Release the mempool snapshot             |

## Support

For technical support and questions:

* Support: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [Ogmios Official Documentation](https://ogmios.dev/)
* [Ogmios JSON-RPC API](../ogmios-json-rpc-api/)
* [Cardano (ADA)](../)
