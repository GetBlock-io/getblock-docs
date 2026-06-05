---
description: >-
  Example code for the LedgerService/GetTransaction gRPC method. Complete guide
  on how to use LedgerService/GetTransaction gRPC method in GetBlock Web3
  documentation.
---

# LedgerService/GetTransaction - SUI

Returns a transaction by its digest, including the full effects, events emitted, and inputs/outputs. The optional `read_mask` lets you fetch just the parts you need — for example, requesting only `effects.status` is significantly cheaper than fetching the full transaction body.

**Service**: `sui.rpc.v2.LedgerService`\
**Proto file**: [`sui/rpc/v2/ledger_service.proto`](https://github.com/MystenLabs/sui-apis/blob/main/proto/sui/rpc/v2/ledger_service.proto)\
**Full method path**: `sui.rpc.v2.LedgerService/GetTransaction`

## Request Fields

| Field      | Type      | Required | Description                                                                      |
| ---------- | --------- | -------- | -------------------------------------------------------------------------------- |
| digest     | string    | Yes      | Transaction digest (Base58)                                                      |
| read\_mask | FieldMask | No       | Field paths — e.g. `["digest", "effects", "events"]`. Use `["*"]` for all fields |

## Request Example

{% tabs %}
{% tab title="grpcurl" %}
```bash
# Clone the official proto files first (one-time setup):
#   git clone https://github.com/MystenLabs/sui-apis.git && cd sui-apis

grpcurl \
  -import-path proto \
  -proto sui/rpc/v2/ledger_service.proto \
  -H "x-grpc-web: 1" \
  -d '{
    "digest": "8WmKqcRkV5JZw8gEjGyzVxRYHnDvKmJsxLBp7t6vNqrA",
    "read_mask": {
        "paths": [
            "*"
        ]
    }
}' \
  go.getblock.io:443/<ACCESS-TOKEN> \
  sui.rpc.v2.LedgerService/GetTransaction
```
{% endtab %}

{% tab title="TypeScript (@grpc/grpc-js)" %}
```typescript
import * as grpc from '@grpc/grpc-js';
import * as protoLoader from '@grpc/proto-loader';
import * as path from 'path';

const PROTO_PATH = path.join(__dirname, 'protos/proto/sui/rpc/v2/ledger_service.proto');
const ACCESS_TOKEN = '<ACCESS-TOKEN>';

const packageDef = protoLoader.loadSync(PROTO_PATH, {
    includeDirs: [path.join(__dirname, 'protos/proto')],
    keepCase: true, longs: String, enums: String, defaults: true,
});
const proto = grpc.loadPackageDefinition(packageDef) as any;
const ServiceClient = proto.sui.rpc.v2.LedgerService;

const metadata = new grpc.Metadata();
metadata.add('authorization', `Bearer ${ACCESS_TOKEN}`);

const client = new ServiceClient('go.getblock.io:443', grpc.credentials.createSsl());

const request = {
    "digest": "8WmKqcRkV5JZw8gEjGyzVxRYHnDvKmJsxLBp7t6vNqrA",
    "read_mask": {
        "paths": [
            "*"
        ]
    }
};

client.GetTransaction(request, metadata, (err: any, response: any) => {
    if (err) {
        console.error('Error:', err);
        return;
    }
    console.log(JSON.stringify(response, null, 2));
});
```
{% endtab %}

{% tab title="Python (grpcio)" %}
```python
# First generate Python stubs from the proto files (one-time setup):
#   pip install grpcio grpcio-tools
#   python -m grpc_tools.protoc -I=proto --python_out=. --grpc_python_out=. \
#       proto/sui/rpc/v2/ledger_service.proto

import grpc
from sui.rpc.v2 import sui_rpc_v2_ledger_service_pb2, sui_rpc_v2_ledger_service_pb2_grpc
from google.protobuf.json_format import ParseDict, MessageToJson

ACCESS_TOKEN = '<ACCESS-TOKEN>'

channel = grpc.secure_channel('go.getblock.io:443', grpc.ssl_channel_credentials())
stub = sui_rpc_v2_ledger_service_pb2_grpc.LedgerServiceStub(channel)

metadata = [('authorization', f'Bearer {ACCESS_TOKEN}')]

# Build the request message from a dict via ParseDict for readability
request_data = {
    "digest": "8WmKqcRkV5JZw8gEjGyzVxRYHnDvKmJsxLBp7t6vNqrA",
    "read_mask": {
        "paths": [
            "*"
        ]
    }
}
request = ParseDict(request_data, sui_rpc_v2_ledger_service_pb2.LedgerRequest())
# (Use the appropriate <Method>Request type — adjust as needed for the specific method.)

response = stub.GetTransaction(request, metadata=metadata)
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
use sui_rpc_v2::ledger_service_client::LedgerServiceClient;
// Plus the relevant <Method>Request type import from sui_rpc_v2.

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let channel = Channel::from_static("https://go.getblock.io").connect().await?;
    let mut client = LedgerServiceClient::with_interceptor(channel, |mut req: Request<()>| {
        req.metadata_mut().insert("authorization", "Bearer <ACCESS-TOKEN>".parse().unwrap());
        Ok(req)
    });

    let request_json = r#"{
    "digest": "8WmKqcRkV5JZw8gEjGyzVxRYHnDvKmJsxLBp7t6vNqrA",
    "read_mask": {
        "paths": [
            "*"
        ]
    }
}"#;
    let request = Request::new(serde_json::from_str(request_json)?);

    let response = client.get_transaction(request).await?;
    println!("Response: {:?}", response.into_inner());
    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Response Example

Responses are encoded in Protocol Buffers binary format on the wire. The example below shows the protobuf JSON encoding for readability.

```json
{
    "transaction": {
        "digest": "8WmKqcRkV5JZw8gEjGyzVxRYHnDvKmJsxLBp7t6vNqrA",
        "transaction": {
            "data": {
                "sender": "0xb871a42470b59c7184033a688f883cf24eb5e66eae1db62319bab27adb30b873",
                "gas_data": {
                    "price": "1000",
                    "budget": "10000000"
                }
            },
            "tx_signatures": [
                "AKx..."
            ]
        },
        "effects": {
            "status": {
                "success": true
            },
            "executed_epoch": "1084",
            "gas_used": {
                "computation_cost": "1000000",
                "storage_cost": "988000",
                "storage_rebate": "977000",
                "non_refundable_storage_fee": "11000"
            },
            "transaction_digest": "8WmKqcRkV5JZw8gEjGyzVxRYHnDvKmJsxLBp7t6vNqrA",
            "created": [],
            "mutated": [
                {
                    "object_id": "0xc8ec1d6e3a7e9d2f5a8c3e7b1d4f9a6c2e5b8d3f7a1c4e9b2d4f6a8c0e2f4b6d",
                    "version": "12345",
                    "digest": "8WmKqcRkV5JZw8gEjGyzVxRYHnDvKmJsxLBp7t6vNqrA"
                }
            ],
            "deleted": []
        },
        "events": [],
        "checkpoint": "260411497",
        "timestamp_ms": "1775050511019"
    }
}
```

## Response Fields

| Field                         | Type               | Description                                                      |
| ----------------------------- | ------------------ | ---------------------------------------------------------------- |
| transaction.digest            | string             | Transaction digest (echoed)                                      |
| transaction.transaction.data  | TransactionData    | The transaction body — sender, gas, kind                         |
| transaction.effects.status    | ExecutionStatus    | `success: true` or `error` with details                          |
| transaction.effects.gas\_used | GasCostSummary     | Computation cost, storage cost, and storage rebate (all in MIST) |
| transaction.effects.mutated   | repeated ObjectRef | Objects modified by this transaction                             |
| transaction.events            | repeated Event     | Move-level events emitted by the transaction                     |
| transaction.checkpoint        | string             | Checkpoint sequence number containing this transaction           |
| transaction.timestamp\_ms     | string             | Block timestamp in milliseconds                                  |

## Use Cases

* Confirming a submitted transaction succeeded after broadcast
* Reading emitted events for indexers
* Computing actual gas cost for accounting
* Building transaction detail views in explorers

## Error Handling

gRPC uses status codes rather than JSON-RPC numeric error codes. The most relevant for this method:

| Status Code         | Numeric | Cause                                                                               |
| ------------------- | ------- | ----------------------------------------------------------------------------------- |
| UNAUTHENTICATED     | 16      | Missing or invalid `<ACCESS-TOKEN>` in the URL path                                 |
| INVALID\_ARGUMENT   | 3       | Request fields are missing, malformed, or fail validation                           |
| UNAVAILABLE         | 14      | Node is overloaded or temporarily unable to handle the request — retry with backoff |
| DEADLINE\_EXCEEDED  | 4       | Request did not complete within the timeout window                                  |
| RESOURCE\_EXHAUSTED | 8       | Rate limit exceeded for your plan                                                   |
| NOT\_FOUND          | 5       | No transaction exists at the requested digest, or it has been pruned from this node |

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
// raw gRPC call to sui.rpc.v2.LedgerService/GetTransaction is available as shown in the
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

# Consult pysui docs for the wrapper of LedgerService.GetTransaction.
```
{% endtab %}
{% endtabs %}
