---
description: >-
  Example code for the ledgerService_batchGetObjects gRPC method. Complete guide
  on how to use ledgerService_batchGetObjects gRPC method in GetBlock Web3
  documentation.
---

# ledgerService\_batchGetObjects - SUI

Returns multiple objects by ID in a single call. More efficient than calling `GetObject` repeatedly when fetching many objects — saves both round trips and per-call overhead. Each entry in the request may have its own `read_mask` for fine-grained field control.

**Service**: `sui.rpc.v2.LedgerService`\
**Proto file**: [`sui/rpc/v2/ledger_service.proto`](https://github.com/MystenLabs/sui-apis/blob/main/proto/sui/rpc/v2/ledger_service.proto)\
**Full method path**: `sui.rpc.v2.LedgerService/BatchGetObjects`

## Request Fields

| Field    | Type                      | Required | Description                                                                                                      |
| -------- | ------------------------- | -------- | ---------------------------------------------------------------------------------------------------------------- |
| requests | repeated GetObjectRequest | Yes      | Array of `GetObjectRequest` entries, each with its own `object_id`, optional `version`, and optional `read_mask` |

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
    "requests": [
        {
            "object_id": "0xc8ec1d6e3a7e9d2f5a8c3e7b1d4f9a6c2e5b8d3f7a1c4e9b2d4f6a8c0e2f4b6d",
            "read_mask": {
                "paths": [
                    "object_id",
                    "version",
                    "object_type"
                ]
            }
        },
        {
            "object_id": "0x1111111111111111111111111111111111111111111111111111111111111111",
            "read_mask": {
                "paths": [
                    "object_id",
                    "version",
                    "object_type"
                ]
            }
        }
    ]
}' \
  go.getblock.io:443/<ACCESS-TOKEN> \
  sui.rpc.v2.LedgerService/BatchGetObjects
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
    "requests": [
        {
            "object_id": "0xc8ec1d6e3a7e9d2f5a8c3e7b1d4f9a6c2e5b8d3f7a1c4e9b2d4f6a8c0e2f4b6d",
            "read_mask": {
                "paths": [
                    "object_id",
                    "version",
                    "object_type"
                ]
            }
        },
        {
            "object_id": "0x1111111111111111111111111111111111111111111111111111111111111111",
            "read_mask": {
                "paths": [
                    "object_id",
                    "version",
                    "object_type"
                ]
            }
        }
    ]
};

client.BatchGetObjects(request, metadata, (err: any, response: any) => {
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
    "requests": [
        {
            "object_id": "0xc8ec1d6e3a7e9d2f5a8c3e7b1d4f9a6c2e5b8d3f7a1c4e9b2d4f6a8c0e2f4b6d",
            "read_mask": {
                "paths": [
                    "object_id",
                    "version",
                    "object_type"
                ]
            }
        },
        {
            "object_id": "0x1111111111111111111111111111111111111111111111111111111111111111",
            "read_mask": {
                "paths": [
                    "object_id",
                    "version",
                    "object_type"
                ]
            }
        }
    ]
}
request = ParseDict(request_data, sui_rpc_v2_ledger_service_pb2.LedgerRequest())
# (Use the appropriate <Method>Request type — adjust as needed for the specific method.)

response = stub.BatchGetObjects(request, metadata=metadata)
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
    "requests": [
        {
            "object_id": "0xc8ec1d6e3a7e9d2f5a8c3e7b1d4f9a6c2e5b8d3f7a1c4e9b2d4f6a8c0e2f4b6d",
            "read_mask": {
                "paths": [
                    "object_id",
                    "version",
                    "object_type"
                ]
            }
        },
        {
            "object_id": "0x1111111111111111111111111111111111111111111111111111111111111111",
            "read_mask": {
                "paths": [
                    "object_id",
                    "version",
                    "object_type"
                ]
            }
        }
    ]
}"#;
    let request = Request::new(serde_json::from_str(request_json)?);

    let response = client.batch_get_objects(request).await?;
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
    "objects": [
        {
            "object_id": "0xc8ec1d6e3a7e9d2f5a8c3e7b1d4f9a6c2e5b8d3f7a1c4e9b2d4f6a8c0e2f4b6d",
            "version": "12345",
            "object_type": "0x2::coin::Coin<0x2::sui::SUI>"
        },
        {
            "object_id": "0x1111111111111111111111111111111111111111111111111111111111111111",
            "version": "67890",
            "object_type": "0x2::nft::Nft"
        }
    ]
}
```
{% endcode %}

## Response Fields

| Field                   | Type            | Description                                                                                     |
| ----------------------- | --------------- | ----------------------------------------------------------------------------------------------- |
| objects                 | repeated Object | Array of objects, one per request entry. Missing or errored entries are still returned in order |
| objects\[].object\_id   | ObjectId        | Object ID                                                                                       |
| objects\[].version      | string          | Object version                                                                                  |
| objects\[].object\_type | string          | Move type                                                                                       |

## Use Cases

* Bulk loading owned-objects pages in wallet UIs
* Indexer enrichment — fetching detailed object data after `ListOwnedObjects` returns IDs
* DEX backends loading multiple pool objects in one call

## Error Handling

gRPC uses status codes rather than JSON-RPC numeric error codes. The most relevant for this method:

| Status Code         | Numeric | Cause                                                                                 |
| ------------------- | ------- | ------------------------------------------------------------------------------------- |
| UNAUTHENTICATED     | 16      | Missing or invalid `<ACCESS-TOKEN>` in the URL path                                   |
| INVALID\_ARGUMENT   | 3       | Request fields are missing, malformed, or fail validation                             |
| UNAVAILABLE         | 14      | Node is overloaded or temporarily unable to handle the request — retry with backoff   |
| DEADLINE\_EXCEEDED  | 4       | Request did not complete within the timeout window                                    |
| RESOURCE\_EXHAUSTED | 8       | Rate limit exceeded for your plan                                                     |
| INVALID\_ARGUMENT   | 3       | Empty `requests` list or too many requests in a single batch (provider-imposed limit) |

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
// raw gRPC call to sui.rpc.v2.LedgerService/BatchGetObjects is available as shown in the
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

# Consult pysui docs for the wrapper of LedgerService.BatchGetObjects.
```
{% endtab %}
{% endtabs %}
