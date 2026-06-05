---
description: >-
  Example code for the LedgerService/GetEpoch gRPC method. Complete guide on how
  to use LedgerService/GetEpoch gRPC method in GetBlock Web3 documentation.
---

# LedgerService/GetEpoch - SUI

Returns information about a specific epoch — validator committee, reference gas price, beginning and ending checkpoints, and total stake. Pass `epoch` for a specific epoch number, or omit for the current epoch.

**Service**: `sui.rpc.v2.LedgerService`\
**Proto file**: [`sui/rpc/v2/ledger_service.proto`](https://github.com/MystenLabs/sui-apis/blob/main/proto/sui/rpc/v2/ledger_service.proto)\
**Full method path**: `sui.rpc.v2.LedgerService/GetEpoch`

## Request Fields

| Field      | Type      | Required | Description                                                                                     |
| ---------- | --------- | -------- | ----------------------------------------------------------------------------------------------- |
| epoch      | uint64    | No       | Epoch number; omit for the current epoch                                                        |
| read\_mask | FieldMask | No       | Field paths — e.g. `["epoch", "reference_gas_price", "validators"]`. Use `["*"]` for all fields |

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
    "epoch": 1084,
    "read_mask": {
        "paths": [
            "*"
        ]
    }
}' \
  go.getblock.io:443/<ACCESS-TOKEN> \
  sui.rpc.v2.LedgerService/GetEpoch
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
    "epoch": 1084,
    "read_mask": {
        "paths": [
            "*"
        ]
    }
};

client.GetEpoch(request, metadata, (err: any, response: any) => {
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
    "epoch": 1084,
    "read_mask": {
        "paths": [
            "*"
        ]
    }
}
request = ParseDict(request_data, sui_rpc_v2_ledger_service_pb2.LedgerRequest())
# (Use the appropriate <Method>Request type — adjust as needed for the specific method.)

response = stub.GetEpoch(request, metadata=metadata)
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
    "epoch": 1084,
    "read_mask": {
        "paths": [
            "*"
        ]
    }
}"#;
    let request = Request::new(serde_json::from_str(request_json)?);

    let response = client.get_epoch(request).await?;
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
    "epoch": {
        "epoch": "1084",
        "committee": {
            "epoch": "1084",
            "members": [
                {
                    "public_key": "AAAA...",
                    "stake_unit": "5000000000"
                }
            ]
        },
        "reference_gas_price": "1000",
        "total_stake": "13000000000000000000",
        "first_checkpoint": "260000000",
        "last_checkpoint": "260411497",
        "start_timestamp_ms": "1774964111019",
        "end_timestamp_ms": "1775050511019",
        "protocol_version": "70"
    }
}
```
{% endcode %}

## Response Fields

| Field                       | Type                   | Description                                                          |
| --------------------------- | ---------------------- | -------------------------------------------------------------------- |
| epoch.epoch                 | string                 | Epoch number                                                         |
| epoch.committee.members     | repeated ValidatorInfo | Validator committee for this epoch                                   |
| epoch.reference\_gas\_price | string                 | Reference gas price for the epoch (MIST)                             |
| epoch.total\_stake          | string                 | Total SUI staked across all validators (MIST)                        |
| epoch.first\_checkpoint     | string                 | First checkpoint sequence number in this epoch                       |
| epoch.last\_checkpoint      | string                 | Last checkpoint sequence number (may be empty for the current epoch) |
| epoch.start\_timestamp\_ms  | string                 | Epoch start timestamp                                                |
| epoch.protocol\_version     | string                 | Protocol version active during this epoch                            |

## Use Cases

* Staking dashboards showing per-epoch validator metrics
* Tracking reference gas price changes between epochs
* Validator monitoring across committee rotations
* Detecting protocol version upgrades

## Error Handling

gRPC uses status codes rather than JSON-RPC numeric error codes. The most relevant for this method:

| Status Code         | Numeric | Cause                                                                                |
| ------------------- | ------- | ------------------------------------------------------------------------------------ |
| UNAUTHENTICATED     | 16      | Missing or invalid `<ACCESS-TOKEN>` in the URL path                                  |
| INVALID\_ARGUMENT   | 3       | Request fields are missing, malformed, or fail validation                            |
| UNAVAILABLE         | 14      | Node is overloaded or temporarily unable to handle the request — retry with backoff  |
| DEADLINE\_EXCEEDED  | 4       | Request did not complete within the timeout window                                   |
| RESOURCE\_EXHAUSTED | 8       | Rate limit exceeded for your plan                                                    |
| NOT\_FOUND          | 5       | Epoch number is above the current epoch, or below the node's earliest retained epoch |

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
// raw gRPC call to sui.rpc.v2.LedgerService/GetEpoch is available as shown in the
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

# Consult pysui docs for the wrapper of LedgerService.GetEpoch.
```
{% endtab %}
{% endtabs %}
