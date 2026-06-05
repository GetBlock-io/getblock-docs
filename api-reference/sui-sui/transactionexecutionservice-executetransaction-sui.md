---
description: >-
  Example code for the TransactionExecutionService/ExecuteTransaction gRPC
  method. Complete guide on how to use
  TransactionExecutionService/ExecuteTransaction gRPC method in GetBlock Web3
  documentation.
---

# TransactionExecutionService/ExecuteTransaction - SUI

Submits a signed transaction for execution and returns the resulting effects. This is the canonical write path for Sui — coin transfers, NFT mints, Move calls, package publishes, all go through here. The transaction must be a BCS-serialized signed transaction blob with one or more signatures.

**Service**: `sui.rpc.v2.TransactionExecutionService`\
**Proto file**: [`sui/rpc/v2/transaction_execution_service.proto`](https://github.com/MystenLabs/sui-apis/blob/main/proto/sui/rpc/v2/transaction_execution_service.proto)\
**Full method path**: `sui.rpc.v2.TransactionExecutionService/ExecuteTransaction`

## Request Fields

| Field       | Type                   | Required | Description                                      |
| ----------- | ---------------------- | -------- | ------------------------------------------------ |
| transaction | Transaction            | Yes      | BCS-encoded signed transaction (as bytes or hex) |
| signatures  | repeated UserSignature | Yes      | One or more signatures over the transaction      |

## Request Example

{% tabs %}
{% tab title="grpcurl" %}
```bash
# Clone the official proto files first (one-time setup):
#   git clone https://github.com/MystenLabs/sui-apis.git && cd sui-apis

grpcurl \
  -import-path proto \
  -proto sui/rpc/v2/transaction_execution_service.proto \
  -H "x-grpc-web: 1" \
  -d '{
    "transaction": {
        "bcs": "AQAA..."
    },
    "signatures": [
        {
            "bcs": "AKx..."
        }
    ]
}' \
  go.getblock.io:443/<ACCESS-TOKEN> \
  sui.rpc.v2.TransactionExecutionService/ExecuteTransaction
```
{% endtab %}

{% tab title="TypeScript (@grpc/grpc-js)" %}
```typescript
import * as grpc from '@grpc/grpc-js';
import * as protoLoader from '@grpc/proto-loader';
import * as path from 'path';

const PROTO_PATH = path.join(__dirname, 'protos/proto/sui/rpc/v2/transaction_execution_service.proto');
const ACCESS_TOKEN = '<ACCESS-TOKEN>';

const packageDef = protoLoader.loadSync(PROTO_PATH, {
    includeDirs: [path.join(__dirname, 'protos/proto')],
    keepCase: true, longs: String, enums: String, defaults: true,
});
const proto = grpc.loadPackageDefinition(packageDef) as any;
const ServiceClient = proto.sui.rpc.v2.TransactionExecutionService;

const metadata = new grpc.Metadata();
metadata.add('authorization', `Bearer ${ACCESS_TOKEN}`);

const client = new ServiceClient('go.getblock.io:443', grpc.credentials.createSsl());

const request = {
    "transaction": {
        "bcs": "AQAA..."
    },
    "signatures": [
        {
            "bcs": "AKx..."
        }
    ]
};

client.ExecuteTransaction(request, metadata, (err: any, response: any) => {
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
#       proto/sui/rpc/v2/transaction_execution_service.proto

import grpc
from sui.rpc.v2 import sui_rpc_v2_transaction_execution_service_pb2, sui_rpc_v2_transaction_execution_service_pb2_grpc
from google.protobuf.json_format import ParseDict, MessageToJson

ACCESS_TOKEN = '<ACCESS-TOKEN>'

channel = grpc.secure_channel('go.getblock.io:443', grpc.ssl_channel_credentials())
stub = sui_rpc_v2_transaction_execution_service_pb2_grpc.TransactionExecutionServiceStub(channel)

metadata = [('authorization', f'Bearer {ACCESS_TOKEN}')]

# Build the request message from a dict via ParseDict for readability
request_data = {
    "transaction": {
        "bcs": "AQAA..."
    },
    "signatures": [
        {
            "bcs": "AKx..."
        }
    ]
}
request = ParseDict(request_data, sui_rpc_v2_transaction_execution_service_pb2.TransactionExecutionRequest())
# (Use the appropriate <Method>Request type — adjust as needed for the specific method.)

response = stub.ExecuteTransaction(request, metadata=metadata)
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
use sui_rpc_v2::transaction_execution_service_client::TransactionExecutionServiceClient;
// Plus the relevant <Method>Request type import from sui_rpc_v2.

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let channel = Channel::from_static("https://go.getblock.io").connect().await?;
    let mut client = TransactionExecutionServiceClient::with_interceptor(channel, |mut req: Request<()>| {
        req.metadata_mut().insert("authorization", "Bearer <ACCESS-TOKEN>".parse().unwrap());
        Ok(req)
    });

    let request_json = r#"{
    "transaction": {
        "bcs": "AQAA..."
    },
    "signatures": [
        {
            "bcs": "AKx..."
        }
    ]
}"#;
    let request = Request::new(serde_json::from_str(request_json)?);

    let response = client.execute_transaction(request).await?;
    println!("Response: {:?}", response.into_inner());
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
    "transaction": {
        "digest": "8WmKqcRkV5JZw8gEjGyzVxRYHnDvKmJsxLBp7t6vNqrA",
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
                    "version": "12345"
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
{% endcode %}

## Response Fields

| Field                         | Type               | Description                                                |
| ----------------------------- | ------------------ | ---------------------------------------------------------- |
| transaction.digest            | string             | Resulting transaction digest                               |
| transaction.effects.status    | ExecutionStatus    | `success: true` or `error` with details                    |
| transaction.effects.gas\_used | GasCostSummary     | Computation cost, storage cost, and storage rebate in MIST |
| transaction.effects.created   | repeated ObjectRef | Newly created objects (e.g. minted NFTs)                   |
| transaction.effects.mutated   | repeated ObjectRef | Objects modified by this transaction                       |
| transaction.effects.deleted   | repeated ObjectRef | Objects deleted by this transaction                        |
| transaction.events            | repeated Event     | Move-level events emitted during execution                 |
| transaction.checkpoint        | string             | Checkpoint sequence number containing this transaction     |

## Use Cases

* Submitting SUI transfers
* Calling Move functions on deployed packages
* Publishing new Move packages
* Minting and transferring NFTs and other Move objects

## Error Handling

gRPC uses status codes rather than JSON-RPC numeric error codes. The most relevant for this method:

| Status Code          | Numeric | Cause                                                                                                                          |
| -------------------- | ------- | ------------------------------------------------------------------------------------------------------------------------------ |
| UNAUTHENTICATED      | 16      | Missing or invalid `<ACCESS-TOKEN>` in the URL path                                                                            |
| INVALID\_ARGUMENT    | 3       | Request fields are missing, malformed, or fail validation                                                                      |
| UNAVAILABLE          | 14      | Node is overloaded or temporarily unable to handle the request — retry with backoff                                            |
| DEADLINE\_EXCEEDED   | 4       | Request did not complete within the timeout window                                                                             |
| RESOURCE\_EXHAUSTED  | 8       | Rate limit exceeded for your plan                                                                                              |
| INVALID\_ARGUMENT    | 3       | Malformed transaction bytes, malformed signature, or wrong number of signatures                                                |
| FAILED\_PRECONDITION | 9       | Transaction execution failed during pre-flight validation (e.g. insufficient gas, invalid object reference, sequence mismatch) |

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
// raw gRPC call to sui.rpc.v2.TransactionExecutionService/ExecuteTransaction is available as shown in the
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

# Consult pysui docs for the wrapper of TransactionExecutionService.ExecuteTransaction.
```
{% endtab %}
{% endtabs %}
