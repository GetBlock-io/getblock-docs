---
description: >-
  Example code for the GetVoteOnHash grpc method. Сomplete guide on how to use
  GetVoteOnHash grpc in GetBlock.io Web3 documentation.
---

# GetVoteOnHash - Polygon gRPC

Returns whether the local Bor node accepts a milestone proposal — given a proposed hash for a block range `[startBlockNumber, endBlockNumber]` and a milestone ID. The response is a boolean: `true` if the local Bor node's computation of the range's hash matches the proposed hash (meaning the local node votes yes on the milestone), `false` otherwise. This is the primary voting input Heimdall uses to determine whether to include a milestone signature.

## Request Parameters

| Parameter          | Type   | Required | Description                                                    |
| ------------------ | ------ | -------- | -------------------------------------------------------------- |
| `startBlockNumber` | uint64 | Yes      | First Bor block in the milestone range (inclusive)             |
| `endBlockNumber`   | uint64 | Yes      | Last Bor block in the milestone range (inclusive)              |
| `hash`             | string | Yes      | The proposed hash to vote on (hex-encoded)                     |
| `milestoneId`      | string | Yes      | Unique milestone identifier (from Heimdall's milestone module) |

## Request Example

{% tabs %}
{% tab title="grpcurl" %}
{% code overflow="wrap" %}
```bash
grpcurl -H 'x-access-token: <ACCESS_TOKEN>' \
        -d '{"startBlockNumber": 90387782, "endBlockNumber": 90387784, "hash": "0x1314b80a2536c8a5853b0ea7dfc1089ee3e51618bda46bd349bb6bfe64f58d10", "milestoneId": "test-milestone-1"}' \
        shared.ap-southeast-1.getblock.io:443 \
        bor.BorApi/GetVoteOnHash
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

const request = {"startBlockNumber": 90387782, "endBlockNumber": 90387784, "hash": "0x1314b80a2536c8a5853b0ea7dfc1089ee3e51618bda46bd349bb6bfe64f58d10", "milestoneId": "test-milestone-1"};

client.GetVoteOnHash(request, metadata, (err, response) => {
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

request = bor_pb2.GetVoteOnHashRequest(
{
    startBlockNumber: 90387782, 
    endBlockNumber: 90387784, 
    hash: "0x1314b80a2536c8a5853b0ea7dfc1089ee3e51618bda46bd349bb6bfe64f58d10",
    milestoneId: "test-milestone-1"
        }
)
response = stub.GetVoteOnHash(request, metadata=metadata)

print(MessageToJson(response, indent=2))
```
{% endcode %}
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

    req := &bor.GetVoteOnHashRequest{
     StartBlockNumber: 90387782, 
    EndBlockNumber: 90387784, 
    Hash: "0x1314b80a2536c8a5853b0ea7dfc1089ee3e51618bda46bd349bb6bfe64f58d10",
    MilestoneId: "test-milestone-1"
    }
    resp, err := client.GetVoteOnHash(ctx, req)
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
    "response": true
}
```
{% endcode %}

{% hint style="info" %}
**H160 and H256 encoding.** In JSON representations of protobuf messages, `common.H160` (20-byte addresses) and `common.H256` (32-byte hashes) are shown here as hex strings for readability. The actual JSON wire format from grpcurl or `MessageToJson` reflects the message structure defined in [`common/common.proto`](https://github.com/0xPolygon/polyproto/blob/main/common/common.proto) — typically a nested object encoding. Applications using the typed generated clients receive `H160`/`H256` as strongly-typed message objects, not strings.
{% endhint %}

## Response Fields

| Field      | Type | Description                                                                                            |
| ---------- | ---- | ------------------------------------------------------------------------------------------------------ |
| `response` | bool | `true` = local node's computed hash matches the proposed hash (vote yes); `false` = mismatch (vote no) |

## Use Cases

* Heimdall milestone acknowledgement voting (the primary consumer)
* Debugging why a milestone failed to reach 2/3 validator acknowledgement
* Cross-node consistency verification during milestone proposition

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
| INVALID\_ARGUMENT (3)   | invalid milestone parameters      | Block range or hash format is malformed                                          |

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
  "startBlockNumber": 90387782, "endBlockNumber": 90387784, "hash": "0x1314b80a2536c8a5853b0ea7dfc1089ee3e51618bda46bd349bb6bfe64f58d10", "milestoneId": "test-milestone-1"
};
const response = await new Promise((resolve, reject) => {
    client.GetVoteOnHash(request, metadata, (err, res) => err ? reject(err) : resolve(res));
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
use bor::GetVoteOnHashRequest;

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

    let request = tonic::Request::new(GetVoteOnHashRequest { /* fields */ });
    let response = client.get_vote_on_hash(request).await?;
    println!("{:?}", response);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}
