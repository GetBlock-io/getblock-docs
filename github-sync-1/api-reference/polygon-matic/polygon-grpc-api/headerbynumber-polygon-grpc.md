---
description: >-
  Example code for the HeaderByNumber grpc method. Сomplete guide on how to use
  HeaderByNumber grpc in GetBlock.io Web3 documentation.
---

# HeaderByNumber - Polygon gRPC

Returns the Ethereum-style block header for a specific block. Accepts the same block reference conventions as `eth_getBlockByNumber`: tags (`"latest"`, `"earliest"`, `"pending"`, `"finalized"`, `"safe"`) or a hex block number (`"0x123abc"`). The response is a `Header` message with every canonical Ethereum header field plus the post-Cancun / post-Prague fields (`baseFee`, `withdrawalsHash`, `blobGasUsed`, `excessBlobGas`, `parentBeaconBlockRoot`, `requestsHash`).

## Request Parameters

| Parameter | Type   | Required | Description                                                                                                            |
| --------- | ------ | -------- | ---------------------------------------------------------------------------------------------------------------------- |
| `number`  | string | Yes      | Block reference — tag (`"latest"`, `"earliest"`, `"pending"`, `"finalized"`, `"safe"`) or hex block number (`"0x..."`) |

## Request Example

{% tabs %}
{% tab title="grpcurl" %}
```bash
grpcurl -H 'x-access-token: <ACCESS-TOKEN>' \
        -d '{"number": "latest"}' \
        shared.ap-southeast-1.getblock.io:443 \
        bor.BorApi/HeaderByNumber
```
{% endtab %}

{% tab title="JavaScript (Node.js)" %}
```javascript
import * as grpc from '@grpc/grpc-js';
import protoLoader from '@grpc/proto-loader';

const GRPC_TARGET = 'shared.ap-southeast-1.getblock.io:443';
const ACCESS_TOKEN = '<ACCESS-TOKEN>';
const PROTO_PATH = './protos/bor/bor.proto';

const packageDefinition = protoLoader.loadSync(PROTO_PATH, {
    keepCase: true,
    longs: String,
    enums: String,
    defaults: true,
    oneofs: true,
    includeDirs: ['./protos'],
});
const proto = grpc.loadPackageDefinition(packageDefinition);
const BorApiClient = proto.bor.BorApi;
const client = new BorApiClient(GRPC_TARGET, grpc.credentials.createSsl());

const metadata = new grpc.Metadata();
metadata.add('x-access-token', ACCESS_TOKEN);

const request = {
    "number": "latest"
};

client.HeaderByNumber(request, metadata, (err, response) => {
    if (err) {
        console.error('gRPC error:', err.code, err.message);
        return;
    }
    console.log(JSON.stringify(response, null, 2));
});
```
{% endtab %}

{% tab title="Python" %}
```python
import grpc
from google.protobuf.json_format import MessageToJson

from bor import bor_pb2, bor_pb2_grpc

GRPC_TARGET = "shared.ap-southeast-1.getblock.io:443"
ACCESS_TOKEN = "<ACCESS-TOKEN>"

credentials = grpc.ssl_channel_credentials()
channel = grpc.secure_channel(GRPC_TARGET, credentials)
stub = bor_pb2_grpc.BorApiStub(channel)

metadata = [("x-access-token", ACCESS_TOKEN)]

request = bor_pb2.GetHeaderByNumberRequest(
    number="latest",
)
response = stub.HeaderByNumber(request, metadata=metadata)

print(MessageToJson(response, indent=2))
```
{% endtab %}

{% tab title="Go" %}
```go
package main

import (
    "context"
    "crypto/tls"
    "encoding/json"
    "fmt"
    "log"
    "time"

    "google.golang.org/grpc"
    "google.golang.org/grpc/credentials"
    "google.golang.org/grpc/metadata"

    "github.com/0xPolygon/polyproto/bor"
)

func main() {
    target := "shared.ap-southeast-1.getblock.io:443"
    accessToken := "<ACCESS-TOKEN>"

    creds := credentials.NewTLS(&tls.Config{})
    conn, err := grpc.Dial(target, grpc.WithTransportCredentials(creds))
    if err != nil {
        log.Fatalf("dial: %v", err)
    }
    defer conn.Close()

    client := bor.NewBorApiClient(conn)

    ctx, cancel := context.WithTimeout(context.Background(), 10*time.Second)
    defer cancel()
    ctx = metadata.AppendToOutgoingContext(ctx, "x-access-token", accessToken)

    req := &bor.GetHeaderByNumberRequest{
        Number: "latest",
    }
    resp, err := client.HeaderByNumber(ctx, req)
    if err != nil {
        log.Fatalf("rpc: %v", err)
    }

    j, _ := json.MarshalIndent(resp, "", "  ")
    fmt.Println(string(j))
}
```
{% endtab %}
{% endtabs %}

## Response Example

{% code overflow="wrap" %}
```json
{
    "header": {
        "number": "76543210",
        "parentHash": "0x47edbdc8da5cd1b3127ea8f1ce5da6739fa39e7e6d2eb1c9b50f1c83de0a98b4",
        "time": "1752624000",
        "uncleHash": "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
        "coinbase": "0x1b2f4b1e1e9c66e6d5e1c72e4c8e7b5a4d3f2c1b0",
        "stateRoot": "0x5eae9c5cbe0e8d3e2f5cd8b0a4e3b1f2c9d8e7f6a5b4c3d2e1f0a9b8c7d6e5f4",
        "txRoot": "0x9c8f0e4f8d2e7c6b5a4f3e2d1c0b9a8e7f6d5c4b3a2b1c9d8e7f6a5b4c3d2e1f",
        "receiptRoot": "0x3a2b1c9d8e7f6a5b4c3d2e1f0a9b8c7d6e5f4a3b2c1d0e9f8a7b6c5d4e3f2a1b",
        "bloom": "AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA==",
        "difficulty": "AQ==",
        "gasLimit": "30000000",
        "gasUsed": "15000000",
        "extraData": "0xd68301040c846765746886676f312e3230856c696e7578",
        "mixDigest": "0x0000000000000000000000000000000000000000000000000000000000000000",
        "nonce": "AAAAAAAAAAA=",
        "baseFee": "AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAABZv5wA"
    }
}
```
{% endcode %}

{% hint style="info" %}
**H160 and H256 encoding.** In JSON representations of protobuf messages, `common.H160` (20-byte addresses) and `common.H256` (32-byte hashes) are shown here as hex strings for readability. The actual JSON wire format from grpcurl or `MessageToJson` reflects the message structure defined in [`common/common.proto`](https://github.com/0xPolygon/polyproto/blob/main/common/common.proto) — typically a nested object encoding. Applications using the typed generated clients receive `H160`/`H256` as strongly-typed message objects, not strings.
{% endhint %}

## Response Fields

| Field                          | Type              | Description                                                                                 |
| ------------------------------ | ----------------- | ------------------------------------------------------------------------------------------- |
| `header`                       | Header            | The block header — see fields below                                                         |
| `header.number`                | uint64            | Block number                                                                                |
| `header.parentHash`            | H256              | Parent block hash                                                                           |
| `header.time`                  | uint64            | Unix timestamp of block production                                                          |
| `header.uncleHash`             | H256              | Hash of the uncles list (always `0x1dcc4de8...` on Polygon PoS — no uncle blocks)           |
| `header.coinbase`              | H160              | Address of the block producer (validator that authored this block)                          |
| `header.stateRoot`             | H256              | Root of the state trie after processing this block                                          |
| `header.txRoot`                | H256              | Root of the transactions trie for this block                                                |
| `header.receiptRoot`           | H256              | Root of the receipts trie for this block                                                    |
| `header.bloom`                 | bytes             | Bloom filter over the logs emitted by this block's transactions                             |
| `header.difficulty`            | bytes             | Consensus difficulty (a compact big-endian representation)                                  |
| `header.gasLimit`              | uint64            | Maximum gas allowed in the block                                                            |
| `header.gasUsed`               | uint64            | Actual gas consumed by transactions in the block                                            |
| `header.extraData`             | bytes             | Arbitrary extra data field — used by Bor to encode producer signatures at sprint boundaries |
| `header.mixDigest`             | H256              | Mix digest (unused on PoS; typically zero)                                                  |
| `header.nonce`                 | bytes             | PoW nonce (unused on PoS; typically zero)                                                   |
| `header.baseFee`               | bytes             | EIP-1559 base fee per gas (compact big-endian)                                              |
| `header.withdrawalsHash`       | H256              | Root of the withdrawals list (post-Shapella)                                                |
| `header.blobGasUsed`           | uint64 (optional) | Blob gas used in the block (post-Cancun)                                                    |
| `header.excessBlobGas`         | uint64 (optional) | Excess blob gas carried into next block (post-Cancun)                                       |
| `header.parentBeaconBlockRoot` | H256              | Beacon block root of the parent (post-Cancun)                                               |
| `header.requestsHash`          | H256              | Requests hash (post-Prague / EIP-7685)                                                      |

## Use Cases

* Verifying block ordering by comparing `parentHash` chains — the primary Heimdall use case
* Reading block metadata (timestamp, gas usage) without downloading the transaction list
* Building a lightweight block observer that streams headers only
* Validating that a producer signed the block via `extraData` decoding

## Error Handling

| gRPC Status (Code)      | Message                           | Cause                                                                                                             |
| ----------------------- | --------------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| UNAUTHENTICATED (16)    | missing or invalid x-access-token | Access token missing from `x-access-token` gRPC metadata or invalid                                               |
| PERMISSION\_DENIED (7)  | access denied                     | Access token doesn't have permission for this method or endpoint                                                  |
| INVALID\_ARGUMENT (3)   | invalid request                   | Request message is malformed or fails validation (e.g. invalid block number tag)                                  |
| INTERNAL (13)           | internal error                    | Server-side error while processing the request                                                                    |
| UNAVAILABLE (14)        | service unavailable               | Bor node is temporarily unavailable (syncing, restart, or overload)                                               |
| DEADLINE\_EXCEEDED (4)  | deadline exceeded                 | Request took longer than the client-configured timeout                                                            |
| RESOURCE\_EXHAUSTED (8) | rate limit exceeded               | Rate limit exceeded for your plan                                                                                 |
| NOT\_FOUND (5)          | block not found                   | Block number is beyond the current chain tip, or `"pending"` requested when no pending block is being constructed |

## SDK Integration

{% tabs %}
{% tab title="TypeScript (ts-proto)" %}
```typescript
// 1. Vendor polyproto and generate typed clients with ts-proto:
//    git clone https://github.com/0xPolygon/polyproto
//    npx protoc \
//      --plugin=./node_modules/.bin/protoc-gen-ts_proto \
//      --ts_proto_out=./src/generated \
//      --ts_proto_opt=outputServices=grpc-js \
//      -I ./polyproto \
//      polyproto/bor/bor.proto polyproto/common/common.proto
//
// 2. Use the generated typed client:

import * as grpc from '@grpc/grpc-js';
import { BorApiClient } from './generated/bor/bor';

const client = new BorApiClient('shared.ap-southeast-1.getblock.io:443', grpc.credentials.createSsl());
const metadata = new grpc.Metadata();
metadata.add('x-access-token', '<ACCESS-TOKEN>');

const request = {
    "number": "latest"
};
const response = await new Promise((resolve, reject) => {
    client.HeaderByNumber(request, metadata, (err, res) => err ? reject(err) : resolve(res));
});
console.log(response);
```
{% endtab %}

{% tab title="Rust (tonic)" %}
```rust
// 1. Vendor polyproto and add tonic-build to build.rs:
//    tonic_build::configure()
//        .compile(&["polyproto/bor/bor.proto"], &["polyproto"])?;
//
// 2. Use the generated typed client:

use tonic::{transport::{Channel, ClientTlsConfig}, Request};

pub mod bor {
    tonic::include_proto!("bor");
}
use bor::bor_api_client::BorApiClient;
use bor::GetHeaderByNumberRequest;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let tls = ClientTlsConfig::new();
    let channel = Channel::from_static("https://shared.ap-southeast-1.getblock.io")
        .tls_config(tls)?
        .connect()
        .await?;

    let mut client = BorApiClient::with_interceptor(channel, |mut req: Request<()>| {
        req.metadata_mut().insert(
            "x-access-token",
            "<ACCESS-TOKEN>".parse().unwrap(),
        );
        Ok(req)
    });

    let request = tonic::Request::new(GetHeaderByNumberRequest { /* fields */ });
    let response = client.header_by_number(request).await?;
    println!("{:?}", response);
    Ok(())
}
```
{% endtab %}
{% endtabs %}
