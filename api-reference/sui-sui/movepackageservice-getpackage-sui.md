---
description: >-
  Example code for the MovePackageService/GetPackage gRPC method. Complete guide
  on how to use MovePackageService/GetPackage gRPC method in GetBlock Web3
  documentation.
---

# MovePackageService/GetPackage - SUI

Returns metadata for a Move package — the modules it contains, their dependencies, and the package version. Move packages on Sui are upgradeable; this method returns the package at its current latest version unless `version` is specified.

**Service**: `sui.rpc.v2.MovePackageService`\
**Proto file**: [`sui/rpc/v2/move_package_service.proto`](https://github.com/MystenLabs/sui-apis/blob/main/proto/sui/rpc/v2/move_package_service.proto)\
**Full method path**: `sui.rpc.v2.MovePackageService/GetPackage`

## Request Fields

| Field       | Type      | Required | Description                                   |
| ----------- | --------- | -------- | --------------------------------------------- |
| package\_id | ObjectId  | Yes      | 32-byte ID of the package                     |
| version     | uint64    | No       | Specific package version; omit for the latest |
| read\_mask  | FieldMask | No       | Field paths to include                        |

## Request Example

{% tabs %}
{% tab title="grpcurl" %}
```bash
# Clone the official proto files first (one-time setup):
#   git clone https://github.com/MystenLabs/sui-apis.git && cd sui-apis

grpcurl \
  -import-path proto \
  -proto sui/rpc/v2/move_package_service.proto \
  -H "x-grpc-web: 1" \
  -d '{
    "package_id": "0x0000000000000000000000000000000000000000000000000000000000000002",
    "read_mask": {
        "paths": [
            "*"
        ]
    }
}' \
  go.getblock.io:443/<ACCESS-TOKEN> \
  sui.rpc.v2.MovePackageService/GetPackage
```
{% endtab %}

{% tab title="TypeScript (@grpc/grpc-js)" %}
```typescript
import * as grpc from '@grpc/grpc-js';
import * as protoLoader from '@grpc/proto-loader';
import * as path from 'path';

const PROTO_PATH = path.join(__dirname, 'protos/proto/sui/rpc/v2/move_package_service.proto');
const ACCESS_TOKEN = '<ACCESS-TOKEN>';

const packageDef = protoLoader.loadSync(PROTO_PATH, {
    includeDirs: [path.join(__dirname, 'protos/proto')],
    keepCase: true, longs: String, enums: String, defaults: true,
});
const proto = grpc.loadPackageDefinition(packageDef) as any;
const ServiceClient = proto.sui.rpc.v2.MovePackageService;

const metadata = new grpc.Metadata();
metadata.add('authorization', `Bearer ${ACCESS_TOKEN}`);

const client = new ServiceClient('go.getblock.io:443', grpc.credentials.createSsl());

const request = {
    "package_id": "0x0000000000000000000000000000000000000000000000000000000000000002",
    "read_mask": {
        "paths": [
            "*"
        ]
    }
};

client.GetPackage(request, metadata, (err: any, response: any) => {
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
#       proto/sui/rpc/v2/move_package_service.proto

import grpc
from sui.rpc.v2 import sui_rpc_v2_move_package_service_pb2, sui_rpc_v2_move_package_service_pb2_grpc
from google.protobuf.json_format import ParseDict, MessageToJson

ACCESS_TOKEN = '<ACCESS-TOKEN>'

channel = grpc.secure_channel('go.getblock.io:443', grpc.ssl_channel_credentials())
stub = sui_rpc_v2_move_package_service_pb2_grpc.MovePackageServiceStub(channel)

metadata = [('authorization', f'Bearer {ACCESS_TOKEN}')]

# Build the request message from a dict via ParseDict for readability
request_data = {
    "package_id": "0x0000000000000000000000000000000000000000000000000000000000000002",
    "read_mask": {
        "paths": [
            "*"
        ]
    }
}
request = ParseDict(request_data, sui_rpc_v2_move_package_service_pb2.MovePackageRequest())
# (Use the appropriate <Method>Request type — adjust as needed for the specific method.)

response = stub.GetPackage(request, metadata=metadata)
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
use sui_rpc_v2::move_package_service_client::MovePackageServiceClient;
// Plus the relevant <Method>Request type import from sui_rpc_v2.

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let channel = Channel::from_static("https://go.getblock.io").connect().await?;
    let mut client = MovePackageServiceClient::with_interceptor(channel, |mut req: Request<()>| {
        req.metadata_mut().insert("authorization", "Bearer <ACCESS-TOKEN>".parse().unwrap());
        Ok(req)
    });

    let request_json = r#"{
    "package_id": "0x0000000000000000000000000000000000000000000000000000000000000002",
    "read_mask": {
        "paths": [
            "*"
        ]
    }
}"#;
    let request = Request::new(serde_json::from_str(request_json)?);

    let response = client.get_package(request).await?;
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
    "package": {
        "package_id": "0x0000000000000000000000000000000000000000000000000000000000000002",
        "version": "1",
        "modules": [
            {
                "name": "coin",
                "type_count": 5,
                "function_count": 12
            },
            {
                "name": "sui",
                "type_count": 1,
                "function_count": 3
            }
        ],
        "type_origin_table": [],
        "linkage_table": []
    }
}
```
{% endcode %}

## Response Fields

| Field                              | Type                | Description                                            |
| ---------------------------------- | ------------------- | ------------------------------------------------------ |
| package.package\_id                | ObjectId            | Echoed package ID                                      |
| package.version                    | string              | Package version                                        |
| package.modules                    | repeated Module     | Modules contained in the package                       |
| package.modules\[].name            | string              | Module name                                            |
| package.modules\[].type\_count     | uint32              | Number of types defined in this module                 |
| package.modules\[].function\_count | uint32              | Number of functions defined in this module             |
| package.type\_origin\_table        | repeated TypeOrigin | Maps types to their originating modules (for upgrades) |
| package.linkage\_table             | repeated Linkage    | External package dependencies                          |

## Use Cases

* Move IDE / tooling introspection
* Building Move package explorers
* Detecting package upgrades
* Static analysis pipelines

## Error Handling

gRPC uses status codes rather than JSON-RPC numeric error codes. The most relevant for this method:

| Status Code         | Numeric | Cause                                                                               |
| ------------------- | ------- | ----------------------------------------------------------------------------------- |
| UNAUTHENTICATED     | 16      | Missing or invalid `<ACCESS-TOKEN>` in the URL path                                 |
| INVALID\_ARGUMENT   | 3       | Request fields are missing, malformed, or fail validation                           |
| UNAVAILABLE         | 14      | Node is overloaded or temporarily unable to handle the request — retry with backoff |
| DEADLINE\_EXCEEDED  | 4       | Request did not complete within the timeout window                                  |
| RESOURCE\_EXHAUSTED | 8       | Rate limit exceeded for your plan                                                   |
| NOT\_FOUND          | 5       | No package exists at the requested ID, or the requested version doesn't exist       |

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
// raw gRPC call to sui.rpc.v2.MovePackageService/GetPackage is available as shown in the
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

# Consult pysui docs for the wrapper of MovePackageService.GetPackage.
```
{% endtab %}
{% endtabs %}
