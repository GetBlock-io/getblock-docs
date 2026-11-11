---
description: >-
  Example code for the GetRootHash grpc method. Сomplete guide on how to use
  GetRootHash grpc in GetBlock.io Web3 documentation.
---

# GetRootHash - Polygon gRPC

Computes the Merkle root over a Bor block range `[startBlockNumber, endBlockNumber]` in the exact form Heimdall uses to build checkpoint submissions to Ethereum L1. This is the equivalent of the `bor_getRootHash` JSON-RPC method as a strongly-typed gRPC call. The root hash is the value Heimdall validators sign and submit to Ethereum via the RootChain contract to anchor Polygon state to L1.

## Request Parameters

| Parameter          | Type   | Required | Description                              |
| ------------------ | ------ | -------- | ---------------------------------------- |
| `startBlockNumber` | uint64 | Yes      | First Bor block in the range (inclusive) |
| `endBlockNumber`   | uint64 | Yes      | Last Bor block in the range (inclusive)  |

## Request Example

{% tabs %}
{% tab title="grpcurl" %}
```bash
grpcurl -H 'x-access-token: <ACCESS-TOKEN>' \
        -d '{"startBlockNumber": "90385120", "endBlockNumber": "90385122"}' \
        shared.ap-southeast-1.getblock.io:443 \
        bor.BorApi/GetRootHash
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
    "startBlockNumber": "90385120",
    "endBlockNumber": "90385122"
};

client.GetRootHash(request, metadata, (err, response) => {
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

request = bor_pb2.GetRootHashRequest(
    startBlockNumber="90385120",
    endBlockNumber="90385122",
)
response = stub.GetRootHash(request, metadata=metadata)

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

    req := &bor.GetRootHashRequest{
        StartBlockNumber: "90385120",
        EndBlockNumber: "90385122",
    }
    resp, err := client.GetRootHash(ctx, req)
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
  "rootHash": "e9135010ac21a8dcdab1c4fd76a6ea1eefd41fab9f33b6cb834b40d8cf2f91f3"
}
```
{% endcode %}

{% hint style="info" %}
**H160 and H256 encoding.** In JSON representations of protobuf messages, `common.H160` (20-byte addresses) and `common.H256` (32-byte hashes) are shown here as hex strings for readability. The actual JSON wire format from grpcurl or `MessageToJson` reflects the message structure defined in [`common/common.proto`](https://github.com/0xPolygon/polyproto/blob/main/common/common.proto) — typically a nested object encoding. Applications using the typed generated clients receive `H160`/`H256` as strongly-typed message objects, not strings.
{% endhint %}

## Response Fields

| Field      | Type   | Description                                |
| ---------- | ------ | ------------------------------------------ |
| `rootHash` | string | Hex-encoded Merkle root of the block range |

## Use Cases

* Heimdall checkpoint construction (the primary consumer)
* Independent verification of checkpoint submissions on Ethereum L1
* Bridge audit tooling — reproducing the L1-submitted root from Bor data

## Error Handling

| gRPC Status (Code)      | Message                           | Cause                                                                                 |
| ----------------------- | --------------------------------- | ------------------------------------------------------------------------------------- |
| UNAUTHENTICATED (16)    | missing or invalid x-access-token | Access token missing from `x-access-token` gRPC metadata or invalid                   |
| PERMISSION\_DENIED (7)  | access denied                     | Access token doesn't have permission for this method or endpoint                      |
| INVALID\_ARGUMENT (3)   | invalid request                   | Request message is malformed or fails validation (e.g. invalid block number tag)      |
| INTERNAL (13)           | internal error                    | Server-side error while processing the request                                        |
| UNAVAILABLE (14)        | service unavailable               | Bor node is temporarily unavailable (syncing, restart, or overload)                   |
| DEADLINE\_EXCEEDED (4)  | deadline exceeded                 | Request took longer than the client-configured timeout                                |
| RESOURCE\_EXHAUSTED (8) | rate limit exceeded               | Rate limit exceeded for your plan                                                     |
| INVALID\_ARGUMENT (3)   | invalid block range               | `endBlockNumber` less than `startBlockNumber`, or range exceeds implementation limits |
| NOT\_FOUND (5)          | block range not available         | One or both endpoint blocks are beyond the current chain tip                          |

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
    "startBlockNumber": "76543000",
    "endBlockNumber": "76543299"
};
const response = await new Promise((resolve, reject) => {
    client.GetRootHash(request, metadata, (err, res) => err ? reject(err) : resolve(res));
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
use bor::GetRootHashRequest;

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

    let request = tonic::Request::new(GetRootHashRequest { /* fields */ });
    let response = client.get_root_hash(request).await?;
    println!("{:?}", response);
    Ok(())
}
```
{% endtab %}
{% endtabs %}
