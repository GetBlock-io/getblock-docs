---
description: >-
  Example code for the tateService/ListDynamicFields gRPC method. Complete guide
  on how to use tateService/ListDynamicFields gRPC method in GetBlock Web3
  documentation.
---

# tateService/ListDynamicFields - SUI

Returns the dynamic fields of a parent object. Dynamic fields are Sui's mechanism for attaching child objects to a parent with arbitrary keys — used for collections, mappings, and on-chain key-value stores. Paginated.

**Service**: `sui.rpc.v2.StateService`\
**Proto file**: [`sui/rpc/v2/state_service.proto`](https://github.com/MystenLabs/sui-apis/blob/main/proto/sui/rpc/v2/state_service.proto)\
**Full method path**: `sui.rpc.v2.StateService/ListDynamicFields`

## Request Fields

| Field       | Type     | Required | Description                               |
| ----------- | -------- | -------- | ----------------------------------------- |
| parent      | ObjectId | Yes      | 32-byte ID of the parent object           |
| page\_size  | uint32   | No       | Maximum fields per page                   |
| page\_token | string   | No       | Pagination token from a previous response |

## Request Example

{% tabs %}
{% tab title="grpcurl" %}
```bash
# Clone the official proto files first (one-time setup):
#   git clone https://github.com/MystenLabs/sui-apis.git && cd sui-apis

grpcurl \
  -import-path proto \
  -proto sui/rpc/v2/state_service.proto \
  -H "x-grpc-web: 1" \
  -d '{
    "parent": "0xc8ec1d6e3a7e9d2f5a8c3e7b1d4f9a6c2e5b8d3f7a1c4e9b2d4f6a8c0e2f4b6d",
    "page_size": 50
}' \
  go.getblock.io:443/<ACCESS-TOKEN> \
  sui.rpc.v2.StateService/ListDynamicFields
```
{% endtab %}

{% tab title="TypeScript (@grpc/grpc-js)" %}
```typescript
import * as grpc from '@grpc/grpc-js';
import * as protoLoader from '@grpc/proto-loader';
import * as path from 'path';

const PROTO_PATH = path.join(__dirname, 'protos/proto/sui/rpc/v2/state_service.proto');
const ACCESS_TOKEN = '<ACCESS-TOKEN>';

const packageDef = protoLoader.loadSync(PROTO_PATH, {
    includeDirs: [path.join(__dirname, 'protos/proto')],
    keepCase: true, longs: String, enums: String, defaults: true,
});
const proto = grpc.loadPackageDefinition(packageDef) as any;
const ServiceClient = proto.sui.rpc.v2.StateService;

const metadata = new grpc.Metadata();
metadata.add('authorization', `Bearer ${ACCESS_TOKEN}`);

const client = new ServiceClient('go.getblock.io:443', grpc.credentials.createSsl());

const request = {
    "parent": "0xc8ec1d6e3a7e9d2f5a8c3e7b1d4f9a6c2e5b8d3f7a1c4e9b2d4f6a8c0e2f4b6d",
    "page_size": 50
};

client.ListDynamicFields(request, metadata, (err: any, response: any) => {
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
#       proto/sui/rpc/v2/state_service.proto

import grpc
from sui.rpc.v2 import sui_rpc_v2_state_service_pb2, sui_rpc_v2_state_service_pb2_grpc
from google.protobuf.json_format import ParseDict, MessageToJson

ACCESS_TOKEN = '<ACCESS-TOKEN>'

channel = grpc.secure_channel('go.getblock.io:443', grpc.ssl_channel_credentials())
stub = sui_rpc_v2_state_service_pb2_grpc.StateServiceStub(channel)

metadata = [('authorization', f'Bearer {ACCESS_TOKEN}')]

# Build the request message from a dict via ParseDict for readability
request_data = {
    "parent": "0xc8ec1d6e3a7e9d2f5a8c3e7b1d4f9a6c2e5b8d3f7a1c4e9b2d4f6a8c0e2f4b6d",
    "page_size": 50
}
request = ParseDict(request_data, sui_rpc_v2_state_service_pb2.StateRequest())
# (Use the appropriate <Method>Request type — adjust as needed for the specific method.)

response = stub.ListDynamicFields(request, metadata=metadata)
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
use sui_rpc_v2::state_service_client::StateServiceClient;
// Plus the relevant <Method>Request type import from sui_rpc_v2.

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let channel = Channel::from_static("https://go.getblock.io").connect().await?;
    let mut client = StateServiceClient::with_interceptor(channel, |mut req: Request<()>| {
        req.metadata_mut().insert("authorization", "Bearer <ACCESS-TOKEN>".parse().unwrap());
        Ok(req)
    });

    let request_json = r#"{
    "parent": "0xc8ec1d6e3a7e9d2f5a8c3e7b1d4f9a6c2e5b8d3f7a1c4e9b2d4f6a8c0e2f4b6d",
    "page_size": 50
}"#;
    let request = Request::new(serde_json::from_str(request_json)?);

    let response = client.list_dynamic_fields(request).await?;
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
    "dynamic_fields": [
        {
            "name": {
                "type": "u64",
                "value": "1"
            },
            "object_id": "0xaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa",
            "object_type": "0x2::dynamic_field::Field<u64, 0x2::coin::Coin<0x2::sui::SUI>>"
        },
        {
            "name": {
                "type": "u64",
                "value": "2"
            },
            "object_id": "0xbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb",
            "object_type": "0x2::dynamic_field::Field<u64, 0x2::coin::Coin<0x2::sui::SUI>>"
        }
    ],
    "next_page_token": ""
}
```
{% endcode %}

## Response Fields

| Field                           | Type                  | Description                                 |
| ------------------------------- | --------------------- | ------------------------------------------- |
| dynamic\_fields                 | repeated DynamicField | Dynamic field entries on the parent         |
| dynamic\_fields\[].name         | MoveValue             | Field key (Move-typed value)                |
| dynamic\_fields\[].object\_id   | ObjectId              | Object ID of the field's child object       |
| dynamic\_fields\[].object\_type | string                | Full Move type of the field                 |
| next\_page\_token               | string                | Pagination cursor; empty when no more pages |

## Use Cases

* Enumerating on-chain mappings (e.g. governance vote tallies, voting registries)
* Walking DEX pools' liquidity bins
* Reading game state stored under dynamic fields

## Error Handling

gRPC uses status codes rather than JSON-RPC numeric error codes. The most relevant for this method:

| Status Code         | Numeric | Cause                                                                               |
| ------------------- | ------- | ----------------------------------------------------------------------------------- |
| UNAUTHENTICATED     | 16      | Missing or invalid `<ACCESS-TOKEN>` in the URL path                                 |
| INVALID\_ARGUMENT   | 3       | Request fields are missing, malformed, or fail validation                           |
| UNAVAILABLE         | 14      | Node is overloaded or temporarily unable to handle the request — retry with backoff |
| DEADLINE\_EXCEEDED  | 4       | Request did not complete within the timeout window                                  |
| RESOURCE\_EXHAUSTED | 8       | Rate limit exceeded for your plan                                                   |

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
// raw gRPC call to sui.rpc.v2.StateService/ListDynamicFields is available as shown in the
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

# Consult pysui docs for the wrapper of StateService.ListDynamicFields.
```
{% endtab %}
{% endtabs %}
