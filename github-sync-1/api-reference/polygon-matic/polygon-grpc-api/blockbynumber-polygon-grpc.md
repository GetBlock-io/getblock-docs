---
description: >-
  Example code for the BlockByNumber grpc method. Сomplete guide on how to use
  BlockByNumber grpc in GetBlock.io Web3 documentation.
---

# BlockByNumber - Polygon gRPC

Returns a `Block` message for a specific block. Per the current `bor.proto` schema, `Block` wraps a `Header` — the message is intentionally header-only at the transport level.&#x20;

Transaction bodies for the block are read via `eth_*` JSON-RPC (`eth_getBlockByNumber` with `full=true`) or via per-transaction lookups. The value of this method over `HeaderByNumber` is the message-type distinction, which lets clients treat `block` and `header` as semantically different types even when the payload is currently identical.

## Request Parameters

| Parameter | Type   | Required | Description                                            |
| --------- | ------ | -------- | ------------------------------------------------------ |
| `number`  | string | Yes      | Block reference — same conventions as `HeaderByNumber` |

## Request Example

{% tabs %}
{% tab title="grpcurl" %}
```bash
grpcurl -H 'x-access-token: <ACCESS-TOKEN>' \
        -d '{"number": "latest"}' \
        shared.ap-southeast-1.getblock.io:443 \
        bor.BorApi/BlockByNumber
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

client.BlockByNumber(request, metadata, (err, response) => {
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

request = bor_pb2.GetBlockByNumberRequest(
    number="latest",
)
response = stub.BlockByNumber(request, metadata=metadata)

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

    req := &bor.GetBlockByNumberRequest{
        Number: "latest",
    }
    resp, err := client.BlockByNumber(ctx, req)
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
    "number": "90384321",
    "parentHash": {
      "Hi": {
        "Hi": "10383105818818211555",
        "Lo": "7546534525976938276"
      },
      "Lo": {
        "Hi": "13826036844615576023",
        "Lo": "17803805182371570277"
      }
    },
    "time": "1784281479",
    "uncleHash": {
      "Hi": {
        "Hi": "2147176784914242938",
        "Lo": "12359484209441330202"
      },
      "Lo": {
        "Hi": "15209294876342121491",
        "Lo": "17339213695834952519"
      }
    },
    "coinbase": {
      "Hi": {}
    },
    "stateRoot": {
      "Hi": {
        "Hi": "4344644223337377376",
        "Lo": "11461457318366706797"
      },
      "Lo": {
        "Hi": "16604064002785543251",
        "Lo": "15476978890853704051"
      }
    },
    "txRoot": {
      "Hi": {
        "Hi": "491814625002787074",
        "Lo": "12637656142692740237"
      },
      "Lo": {
        "Hi": "7897856382903470334",
        "Lo": "13117801461521437071"
      }
    },
    "receiptRoot": {
      "Hi": {
        "Hi": "5719137103850093152",
        "Lo": "16110631761880534114"
      },
      "Lo": {
        "Hi": "15417378640221180198",
        "Lo": "8378727512741557667"
      }
    },
    "bloom": "X7EQssZyHKwCBF0TtgrWsKGRaEneWQxHKfCRbA3GWBEk5dUstXDbkAHPmpAskRNTmZCQJEhCc07uSEGTgTwuQEAKgRhSvMeaIZDXaqYygKgjqEZgmQMgWyEjKZtWQsABbgwo4Eoo5QAhHxgJW+iZFAYtodh6FHFTsUxXVMvqrvUAQSlSXJphNj8s3ywW4NsQSvXXyR/wOmlRFVJQgSAymmq2c58fEAOkjBc6ITgh5yNKbGkYSRWmQRAJEkfQj07FwEA1YivDvEdGr9lL8m8TlmekPm7UD4DRldLBGBdGNpRmVybI1VJspYQbhAAAsLkkJxcqV9SNDWObUwyNknNoQQ==",
    "difficulty": "AQ==",
    "gasLimit": "160000000",
    "gasUsed": "25050575",
    "extraData": "14MBEAiDYm9yiGdvMS4yNi4zhWxpbnV4AAAAAAAAAAD4woD4ucDBgMEBwQLBA8DAwQbBB8EIwQnBCsEKwQvAwQ7BDMDBEcENwRPBFMIQFcEWwRfBF8EYwRrBG8Ecwh0ZwR7BH8EgwSHBIsEEwSPBJcEkwMEPwSbBKsGAwSnBLcEuwS/BMMExwTLBM8E0wTXBNsE3wTjBOcE6wTvBPME9wT7BP8FAwUHBQsFDwUTBRcFGwMFAwUfBSsDBR8FNwUnBK8DBUcFSwSzBUMFQwVXBVsJXWMJOWcEnwMJaT8FahAFuNgBA4FlpuJcW6TXw5LnbSnNxcIAIggsHREgXwVxbAu9KBd8Ly2yTNCoPpwmuYY5qBgyCU8IJ1JRhCMrsQ5WDcykOdQA=",
    "mixDigest": {
      "Hi": {},
      "Lo": {}
    },
    "nonce": "AAAAAAAAAAA=",
    "baseFee": "OcM98YE="
  }
}
```
{% endcode %}

{% hint style="info" %}
**H160 and H256 encoding.** In JSON representations of protobuf messages, `common.H160` (20-byte addresses) and `common.H256` (32-byte hashes) are shown here as hex strings for readability. The actual JSON wire format from grpcurl or `MessageToJson` reflects the message structure defined in [`common/common.proto`](https://github.com/0xPolygon/polyproto/blob/main/common/common.proto) — typically a nested object encoding. Applications using the typed generated clients receive `H160`/`H256` as strongly-typed message objects, not strings.
{% endhint %}

## Response Fields

| Field          | Type   | Description                                              |
| -------------- | ------ | -------------------------------------------------------- |
| `block`        | Block  | The block wrapper                                        |
| `block.header` | Header | The block header — same shape as `HeaderByNumber.header` |

## Use Cases

* Type-safe distinction between `block` and `header` concepts in strongly-typed clients
* Future compatibility if the `Block` message is later extended with transactions or additional metadata

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
| NOT\_FOUND (5)          | block not found                   | Block number is beyond the current chain tip                                     |

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
    "number": "latest"
};
const response = await new Promise((resolve, reject) => {
    client.BlockByNumber(request, metadata, (err, res) => err ? reject(err) : resolve(res));
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
use bor::GetBlockByNumberRequest;

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

    let request = tonic::Request::new(GetBlockByNumberRequest { /* fields */ });
    let response = client.block_by_number(request).await?;
    println!("{:?}", response);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}
