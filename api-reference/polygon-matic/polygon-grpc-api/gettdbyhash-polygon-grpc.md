---
description: >-
  Example code for the GetTdByHash grpc method. Сomplete guide on how to use
  GetTdByHash grpc in GetBlock.io Web3 documentation.
---

# GetTdByHash - Polygon gRPC

Returns the total difficulty (TD) for a block identified by its hash. Total difficulty is the cumulative consensus difficulty from genesis to this block — Bor uses TD for fork-choice: the canonical chain is the one with the highest cumulative TD, with in-turn producer blocks scoring higher than out-of-turn blocks.

## Request Parameters

| Parameter | Type | Required | Description                  |
| --------- | ---- | -------- | ---------------------------- |
| `hash`    | H256 | Yes      | Block hash to look up TD for |

## Request Example

{% tabs %}
{% tab title="grpcurl" %}
{% code overflow="wrap" %}
```bash
grpcurl -H 'x-access-token: <ACCESS_TOKEN>' \
        -d '{"hash": {"Hi": {"Hi": "3272624667754571407", "Lo": "4672811091460113517"}, "Lo": {"Hi": "5134667498046831133", "Lo": "17102463161894732817"}}}' \
        shared.ap-southeast-1.getblock.io:443 \
        bor.BorApi/GetTdByHash

```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Node.js)" %}
{% code overflow="wrap" %}
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

const request = {"hash": {"Hi": {"Hi": "3272624667754571407", "Lo": "4672811091460113517"}, "Lo": {"Hi": "5134667498046831133", "Lo": "17102463161894732817"}}};

client.GetTdByHash(request, metadata, (err, response) => {
    if (err) {
        console.error('gRPC error:', err.code, err.message);
        return;
    }
    console.log(JSON.stringify(response, null, 2));
});
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code overflow="wrap" %}
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

request = bor_pb2.GetTdByHashRequest(
    hash={"Hi": {"Hi": "3272624667754571407", "Lo": "4672811091460113517"}, "Lo": {"Hi": "5134667498046831133", "Lo": "17102463161894732817"}}},
)
response = stub.GetTdByHash(request, metadata=metadata)

print(MessageToJson(response, indent=2))
```
{% endcode %}
{% endtab %}

{% tab title="Go" %}
{% code overflow="wrap" %}
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

    req := &bor.GetTdByHashRequest{
    Hash: {"Hi": {"Hi": "3272624667754571407", "Lo": "4672811091460113517"}, "Lo": {"Hi": "5134667498046831133", "Lo": "17102463161894732817"}}}
    }
    resp, err := client.GetTdByHash(ctx, req)
    if err != nil {
        log.Fatalf("rpc: %v", err)
    }

    j, _ := json.MarshalIndent(resp, "", "  ")
    fmt.Println(string(j))
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Example

{% code overflow="wrap" %}
```json
{
  "totalDifficulty": "1506972941"
}
```
{% endcode %}

{% hint style="info" %}
**H160 and H256 encoding.** In JSON representations of protobuf messages, `common.H160` (20-byte addresses) and `common.H256` (32-byte hashes) are shown here as hex strings for readability. The actual JSON wire format from grpcurl or `MessageToJson` reflects the message structure defined in [`common/common.proto`](https://github.com/0xPolygon/polyproto/blob/main/common/common.proto) — typically a nested object encoding. Applications using the typed generated clients receive `H160`/`H256` as strongly-typed message objects, not strings.
{% endhint %}

## Response Fields

| Field             | Type   | Description                                                |
| ----------------- | ------ | ---------------------------------------------------------- |
| `totalDifficulty` | uint64 | Cumulative consensus difficulty from genesis to this block |

## Use Cases

* Fork detection — comparing TD of competing chain tips
* Chain reorganization analysis
* Verifying that a proposed chain segment has higher cumulative difficulty than the current canonical chain

## Error Handling

| gRPC Status (Code)      | Message                           | Cause                                                                            |
| ----------------------- | --------------------------------- | -------------------------------------------------------------------------------- |
| UNAUTHENTICATED (16)    | missing or invalid x-access-token | Access token missing from `x-access-token` gRPC metadata or invalid              |
| PERMISSION\_DENIED (7)  | access denied                     | Access token doesn't have permission for this method or endpoint                 |
| INVALID\_ARGUMENT (3)   | invalid request                   | Request message is malformed or fails validation (e.g. invalid block number tag) |
| INTERNAL (13)           | internal error                    | Server-side error while processing the request                                   |
| UNAVAILABLE (14)        | service unavailable               | Bor node is temporarily unavailable (syncing, restart, or overload)              |
| DEADLINE\_EXCEEDED (4)  | deadline exceeded                 | Request took longer than the client-configured timeout                           |
| RESOURCE\_EXHAUSTED (8) | rate limit exceeded               | Rate limit exceeded for your plan                                                |
| NOT\_FOUND (5)          | block not found                   | Block hash doesn't correspond to a known block                                   |

## SDK Integration

{% tabs %}
{% tab title="TypeScript (ts-proto)" %}
{% code overflow="wrap" %}
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
        hash={"Hi": {"Hi": "3272624667754571407", "Lo": "4672811091460113517"}, "Lo": {"Hi": "5134667498046831133", "Lo": "17102463161894732817"}}},
};
const response = await new Promise((resolve, reject) => {
    client.GetTdByHash(request, metadata, (err, res) => err ? reject(err) : resolve(res));
});
console.log(response);
```
{% endcode %}
{% endtab %}

{% tab title="Rust (tonic)" %}
{% code overflow="wrap" %}
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
use bor::GetTdByHashRequest;

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

    let request = tonic::Request::new(GetTdByHashRequest { /* fields */ });
    let response = client.get_td_by_hash(request).await?;
    println!("{:?}", response);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}
