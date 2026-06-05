---
description: >-
  Example code for the SubscriptionService/SubscribeCheckpoints gRPC method.
  Complete guide on how to use SubscriptionService/SubscribeCheckpoints gRPC
  method in GetBlock Web3 documentation.
---

# SubscriptionService/SubscribeCheckpoints - SUI

Subscribes to the stream of executed checkpoints. When the subscription initializes, the server emits the latest executed checkpoint it has seen, then continues emitting subsequent checkpoints in order without gaps. If the subscription terminates (client-side cancellation, server-side close, or a connection break), clients can reinitialize and use `GetCheckpoint` / `BatchGetTransactions` to backfill any checkpoints they missed.

{% hint style="info" %}
**Server-side streaming RPC.** This method returns a `stream` of responses rather than a single response. The client opens the call, the server emits messages as events occur, and the stream stays open until the client cancels or the connection terminates. This replaces JSON-RPC WebSocket subscriptions.
{% endhint %}

**Service**: `sui.rpc.v2.SubscriptionService`\
**Proto file**: [`sui/rpc/v2/subscription_service.proto`](https://github.com/MystenLabs/sui-apis/blob/main/proto/sui/rpc/v2/subscription_service.proto)\
**Full method path**: `sui.rpc.v2.SubscriptionService/SubscribeCheckpoints`

## Request Fields

| Field      | Type      | Required | Description                                                                                                                         |
| ---------- | --------- | -------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| read\_mask | FieldMask | No       | Field paths to include in each streamed response (e.g. `["checkpoint.sequence_number", "checkpoint.digest"]` for lightweight feeds) |

## Request Example

{% tabs %}
{% tab title="grpcurl" %}
```bash
# Clone the official proto files first (one-time setup):
#   git clone https://github.com/MystenLabs/sui-apis.git && cd sui-apis

# This is a server-side STREAMING method — grpcurl will keep printing
# responses until you interrupt with Ctrl+C.
grpcurl \
  -import-path proto \
  -proto sui/rpc/v2/subscription_service.proto \
  -H "x-grpc-web: 1" \
  -d '{
    "read_mask": {
        "paths": [
            "checkpoint.sequence_number",
            "checkpoint.digest",
            "checkpoint.summary.timestamp_ms"
        ]
    }
}' \
  go.getblock.io:443/<ACCESS-TOKEN> \
  sui.rpc.v2.SubscriptionService/SubscribeCheckpoints
```
{% endtab %}

{% tab title="TypeScript (@grpc/grpc-js)" %}
```typescript
import * as grpc from '@grpc/grpc-js';
import * as protoLoader from '@grpc/proto-loader';
import * as path from 'path';

const PROTO_PATH = path.join(__dirname, 'protos/proto/sui/rpc/v2/subscription_service.proto');
const ACCESS_TOKEN = '<ACCESS-TOKEN>';

const packageDef = protoLoader.loadSync(PROTO_PATH, {
    includeDirs: [path.join(__dirname, 'protos/proto')],
    keepCase: true, longs: String, enums: String, defaults: true,
});
const proto = grpc.loadPackageDefinition(packageDef) as any;
const ServiceClient = proto.sui.rpc.v2.SubscriptionService;

const metadata = new grpc.Metadata();
metadata.add('authorization', `Bearer ${ACCESS_TOKEN}`);

const client = new ServiceClient('go.getblock.io:443', grpc.credentials.createSsl());

const request = {
    "read_mask": {
        "paths": [
            "checkpoint.sequence_number",
            "checkpoint.digest",
            "checkpoint.summary.timestamp_ms"
        ]
    }
};

const call = client.SubscribeCheckpoints(request, metadata);

call.on('data', (response: any) => {
    console.log('Received:', JSON.stringify(response, null, 2));
});

call.on('end', () => console.log('Stream ended.'));
call.on('error', (err: any) => console.error('Stream error:', err));
```
{% endtab %}

{% tab title="Python (grpcio)" %}
```python
# First generate Python stubs from the proto files (one-time setup):
#   pip install grpcio grpcio-tools
#   python -m grpc_tools.protoc -I=proto --python_out=. --grpc_python_out=. \
#       proto/sui/rpc/v2/subscription_service.proto

import grpc
from sui.rpc.v2 import sui_rpc_v2_subscription_service_pb2, sui_rpc_v2_subscription_service_pb2_grpc
from google.protobuf.json_format import ParseDict, MessageToJson

ACCESS_TOKEN = '<ACCESS-TOKEN>'

channel = grpc.secure_channel('go.getblock.io:443', grpc.ssl_channel_credentials())
stub = sui_rpc_v2_subscription_service_pb2_grpc.SubscriptionServiceStub(channel)

metadata = [('authorization', f'Bearer {ACCESS_TOKEN}')]

# Build the request message from a dict via ParseDict for readability
request_data = {
    "read_mask": {
        "paths": [
            "checkpoint.sequence_number",
            "checkpoint.digest",
            "checkpoint.summary.timestamp_ms"
        ]
    }
}
request = ParseDict(request_data, sui_rpc_v2_subscription_service_pb2.SubscriptionRequest())
# (Use the appropriate <Method>Request type — adjust as needed for the specific method.)

for response in stub.SubscribeCheckpoints(request, metadata=metadata):
    print(response)
```
{% endtab %}

{% tab title="Rust (tonic)" %}
```rust
// Add to Cargo.toml:
//   tonic = "0.12"
//   prost = "0.13"
//   tokio = { version = "1", features = ["full"] }
// Plus a build.rs invoking tonic_build on the Sui proto files.

use tonic::{transport::Channel, Request};
use sui_rpc_v2::subscription_service_client::SubscriptionServiceClient;
// Plus the relevant <Method>Request type import from sui_rpc_v2.

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let channel = Channel::from_static("https://go.getblock.io").connect().await?;
    let mut client = SubscriptionServiceClient::with_interceptor(channel, |mut req: Request<()>| {
        req.metadata_mut().insert("authorization", "Bearer <ACCESS-TOKEN>".parse().unwrap());
        Ok(req)
    });

    let request_json = r#"{
    "read_mask": {
        "paths": [
            "checkpoint.sequence_number",
            "checkpoint.digest",
            "checkpoint.summary.timestamp_ms"
        ]
    }
}"#;
    let request = Request::new(serde_json::from_str(request_json)?);

    let mut stream = client.subscribe_checkpoints(request).await?.into_inner();

    while let Some(response) = stream.message().await? {
        println!("Received: {:?}", response);
    }
    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Response Example

Responses are encoded in Protocol Buffers binary format on the wire. The example below shows the protobuf JSON encoding for readability.

{% code overflow="wrap" %}
```json
{
    "checkpoint": {
        "sequence_number": "260411497",
        "digest": "8WmKqcRkV5JZw8gEjGyzVxRYHnDvKmJsxLBp7t6vNqrA",
        "summary": {
            "timestamp_ms": "1775050511019"
        }
    }
}
```
{% endcode %}

## Response Fields

| Field                            | Type       | Description                                                                        |
| -------------------------------- | ---------- | ---------------------------------------------------------------------------------- |
| checkpoint                       | Checkpoint | Streamed checkpoint object (same schema as `LedgerService.GetCheckpoint` response) |
| checkpoint.sequence\_number      | string     | Checkpoint sequence number — guaranteed to increase monotonically without gaps     |
| checkpoint.digest                | string     | Checkpoint digest                                                                  |
| checkpoint.summary.timestamp\_ms | string     | Checkpoint timestamp                                                               |

## Use Cases

* Real-time indexers consuming the checkpoint stream
* Block explorers showing live chain progression
* Wallets monitoring incoming transactions in near-real-time
* Cross-chain bridge relayers reacting to checkpoint finalization

## Error Handling

gRPC uses status codes rather than JSON-RPC numeric error codes. The most relevant for this method:

| Status Code         | Numeric | Cause                                                                                                  |
| ------------------- | ------- | ------------------------------------------------------------------------------------------------------ |
| UNAUTHENTICATED     | 16      | Missing or invalid `<ACCESS-TOKEN>` in the URL path                                                    |
| INVALID\_ARGUMENT   | 3       | Request fields are missing, malformed, or fail validation                                              |
| UNAVAILABLE         | 14      | Node is overloaded or temporarily unable to handle the request — retry with backoff                    |
| DEADLINE\_EXCEEDED  | 4       | Request did not complete within the timeout window                                                     |
| RESOURCE\_EXHAUSTED | 8       | Rate limit exceeded for your plan                                                                      |
| CANCELLED           | 1       | Client cancelled the stream                                                                            |
| UNAVAILABLE         | 14      | Connection broken or server unable to maintain the stream — reconnect and backfill via `GetCheckpoint` |

## SDK Integration

{% tabs %}
{% tab title="@mysten/sui (TypeScript)" %}
```typescript
// The Mysten Labs TypeScript SDK provides higher-level wrappers around the
// gRPC API. Use @mysten/sui for typed client access:

import { SuiClient } from '@mysten/sui/client';

const client = new SuiClient({
    url: 'https://go.getblock.io/<ACCESS-TOKEN>/'
});

// Refer to the @mysten/sui API reference for the typed wrapper of this method;
// raw gRPC call to sui.rpc.v2.SubscriptionService/SubscribeCheckpoints is available as shown in the
// Request Example tabs above.
```
{% endtab %}

{% tab title="pysui (Python)" %}
```python
# pysui is the community-maintained Python SDK for Sui (Mysten Labs does not
# yet publish an official Python SDK). For raw gRPC access, use grpcio with
# the generated proto stubs as shown in the Request Example tabs above.

from pysui import SuiConfig, SyncClient

config = SuiConfig.user_config(
    rpc_url='https://go.getblock.io/<ACCESS-TOKEN>/'
)
client = SyncClient(config)

# Consult pysui docs for the wrapper of SubscriptionService.SubscribeCheckpoints.
```
{% endtab %}
{% endtabs %}
