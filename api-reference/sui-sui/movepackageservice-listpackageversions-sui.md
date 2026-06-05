---
description: >-
  Example code for the MovePackageService/ListPackageVersions gRPC method.
  Complete guide on how to use MovePackageService/ListPackageVersions gRPC
  method in GetBlock Web3 documentation.
---

# MovePackageService/ListPackageVersions - SUI

Returns all versions of a Move package. Move packages on Sui can be upgraded; each upgrade produces a new package object with an incremented version. This method returns the history.

**Service**: `sui.rpc.v2.MovePackageService`\
**Proto file**: [`sui/rpc/v2/move_package_service.proto`](https://github.com/MystenLabs/sui-apis/blob/main/proto/sui/rpc/v2/move_package_service.proto)\
**Full method path**: `sui.rpc.v2.MovePackageService/ListPackageVersions`

## Request Fields

| Field       | Type     | Required | Description                                                       |
| ----------- | -------- | -------- | ----------------------------------------------------------------- |
| package\_id | ObjectId | Yes      | 32-byte ID of any version of the package (typically the original) |
| page\_size  | uint32   | No       | Maximum versions per page                                         |
| page\_token | string   | No       | Pagination token from a previous response                         |

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
    "page_size": 20
}' \
  go.getblock.io:443/<ACCESS-TOKEN> \
  sui.rpc.v2.MovePackageService/ListPackageVersions
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
    "page_size": 20
};

client.ListPackageVersions(request, metadata, (err: any, response: any) => {
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
    "page_size": 20
}
request = ParseDict(request_data, sui_rpc_v2_move_package_service_pb2.MovePackageRequest())
# (Use the appropriate <Method>Request type — adjust as needed for the specific method.)

response = stub.ListPackageVersions(request, metadata=metadata)
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
    "page_size": 20
}"#;
    let request = Request::new(serde_json::from_str(request_json)?);

    let response = client.list_package_versions(request).await?;
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
    "versions": [
        {
            "package_id": "0x0000000000000000000000000000000000000000000000000000000000000002",
            "version": "1",
            "previous_transaction": "8WmKqcRkV5JZw8gEjGyzVxRYHnDvKmJsxLBp7t6vNqrA"
        },
        {
            "package_id": "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
            "version": "2",
            "previous_transaction": "Q9LkBpZHvJxMsnNzVxRYHnDvKmJsxLBp7t6vNqRAmK"
        }
    ],
    "next_page_token": ""
}
```
{% endcode %}

## Response Fields

| Field                             | Type                    | Description                                                    |
| --------------------------------- | ----------------------- | -------------------------------------------------------------- |
| versions                          | repeated PackageVersion | Package versions, ordered by version number                    |
| versions\[].package\_id           | ObjectId                | Object ID of this version (each upgrade produces a new object) |
| versions\[].version               | string                  | Version number                                                 |
| versions\[].previous\_transaction | string                  | Digest of the transaction that published this version          |
| next\_page\_token                 | string                  | Pagination cursor; empty when no more pages                    |

## Use Cases

* Auditing package upgrade history
* Detecting compromised packages by tracking unexpected upgrades
* Tooling that needs to pin against a specific package version

## Error Handling

gRPC uses status codes rather than JSON-RPC numeric error codes. The most relevant for this method:

| Status Code         | Numeric | Cause                                                                               |
| ------------------- | ------- | ----------------------------------------------------------------------------------- |
| UNAUTHENTICATED     | 16      | Missing or invalid `<ACCESS-TOKEN>` in the URL path                                 |
| INVALID\_ARGUMENT   | 3       | Request fields are missing, malformed, or fail validation                           |
| UNAVAILABLE         | 14      | Node is overloaded or temporarily unable to handle the request — retry with backoff |
| DEADLINE\_EXCEEDED  | 4       | Request did not complete within the timeout window                                  |
| RESOURCE\_EXHAUSTED | 8       | Rate limit exceeded for your plan                                                   |
| NOT\_FOUND          | 5       | Package does not exist                                                              |

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
// raw gRPC call to sui.rpc.v2.MovePackageService/ListPackageVersions is available as shown in the
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

# Consult pysui docs for the wrapper of MovePackageService.ListPackageVersions.
```
{% endtab %}
{% endtabs %}
