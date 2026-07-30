---
description: >-
  Example code for the TransactionReceipt grpc method. Сomplete guide on how to
  use TransactionReceipt grpc in GetBlock.io Web3 documentation.
---

# TransactionReceipt - Polygon gRPC

Returns the receipt for a specific transaction hash — status (success/reverted), gas used, logs emitted, and post-EIP-4844 blob-related fields. Same semantic contract as `eth_getTransactionReceipt` on Bor JSON-RPC, but delivered as a strongly-typed protobuf message.

## Request Parameters

| Parameter | Type | Required | Description                 |
| --------- | ---- | -------- | --------------------------- |
| `hash`    | H256 | Yes      | Transaction hash to look up |

## Request Example

{% tabs %}
{% tab title="grpcurl" %}
{% code overflow="wrap" %}
```bash
grpcurl -H 'x-access-token: <ACCESS_TOKEN>' \
        -d '{"hash": {"Hi": {"Hi": "14862281939317677951", "Lo": "6963111081983623462"}, "Lo": {"Hi": "14023204965242737735", "Lo": "58585669732858373"}}}' \
        shared.ap-southeast-1.getblock.io:443 \
        bor.BorApi/TransactionReceipt
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

const request = {"hash": {"Hi": {"Hi": "14862281939317677951", "Lo": "6963111081983623462"}, "Lo": {"Hi": "14023204965242737735", "Lo": "58585669732858373"}}};

client.TransactionReceipt(request, metadata, (err, response) => {
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

request = bor_pb2.ReceiptRequest(
    hash={"Hi": {"Hi": "14862281939317677951", "Lo": "6963111081983623462"}, "Lo": {"Hi": "14023204965242737735", "Lo": "58585669732858373"}},
)
response = stub.TransactionReceipt(request, metadata=metadata)

print(MessageToJson(response, indent=2))
```
{% endcode %}
{% endtab %}

{% tab title="Go" %}
{% code overflow="wrap" %}
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

    req := &bor.ReceiptRequest{Hash: {"Hi": {"Hi": "14862281939317677951", "Lo": "6963111081983623462"}, "Lo": {"Hi": "14023204965242737735", "Lo": "58585669732858373"}}}
    resp, err := client.TransactionReceipt(ctx, req)
    if err != nil {
        log.Fatalf("rpc: %v", err)
    }

    j, _ := json.MarshalIndent(resp, "", "  ")
    fmt.Println(string(j))
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Example

{% code overflow="wrap" %}
```json
{
  "receipt": {
    "type": "2",
    "status": "1",
    "cumulativeGasUsed": "105645",
    "bloom": {
      "bloom": "AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAABAAAAAAAAAAAAAAAAAAAACAAAAIAAAAAAAAAAAAAAAAAAEAAAACAAAACAAQgIAAAAAAAAAAAAAFAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAgAAAEAAAAAAAAAAAAAAAACIAAAAEAIAAAAAAAAAAAAAAAAAAAAAAACAAAAAAAAAABBAAAAAAAAAAAAAAAAAAAQAAAAAAAAhAAAAwAgECAAAACQAAAAAAAAAAAEAAAAAAABAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAABAAAA=="
    },
    "logs": [
      {
        "address": {
          "Hi": {
            "Hi": "16956573368727236338",
            "Lo": "1575147110570402303"
          },
          "Lo": 2152337153
        },
        "topics": [
          {
            "Hi": {
              "Hi": "15992936130196719771",
              "Lo": "7620847484418887082"
            },
            "Lo": {
              "Hi": "10748869590852608278",
              "Lo": "2951364421682967535"
            }
          },
          {
            "Hi": {
              "Lo": "134172166"
            },
            "Lo": {
              "Hi": "9682146095069244014",
              "Lo": "14890311799781050253"
            }
          },
          {
            "Hi": {
              "Lo": "3135307059"
            },
            "Lo": {
              "Hi": "10204824415584994316",
              "Lo": "3399674193463540339"
            }
          }
        ],
        "data": "AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAS39O5Y=",
        "blockNumber": "90386048",
        "txHash": {
          "Hi": {
            "Hi": "14862281939317677951",
            "Lo": "6963111081983623462"
          },
          "Lo": {
            "Hi": "14023204965242737735",
            "Lo": "58585669732858373"
          }
        },
        "blockHash": {
          "Hi": {
            "Hi": "3272624667754571407",
            "Lo": "4672811091460113517"
          },
          "Lo": {
            "Hi": "5134667498046831133",
            "Lo": "17102463161894732817"
          }
        }
      },
      {
        "address": {
          "Hi": {
            "Hi": "576265067257783443",
            "Lo": "4902637062259540912"
          },
          "Lo": 2361509773
        },
        "topics": [
          {
            "Hi": {
              "Hi": "8436189553101673195",
              "Lo": "4247389624028040444"
            },
            "Lo": {
              "Hi": "6612781626668925314",
              "Lo": "1543707648833898258"
            }
          },
          {
            "Hi": {
              "Lo": "3135307059"
            },
            "Lo": {
              "Hi": "10204824415584994316",
              "Lo": "3399674193463540339"
            }
          },
          {
            "Hi": {},
            "Lo": {
              "Lo": "5066537878"
            }
          }
        ],
        "blockNumber": "90386048",
        "txHash": {
          "Hi": {
            "Hi": "14862281939317677951",
            "Lo": "6963111081983623462"
          },
          "Lo": {
            "Hi": "14023204965242737735",
            "Lo": "58585669732858373"
          }
        },
        "blockHash": {
          "Hi": {
            "Hi": "3272624667754571407",
            "Lo": "4672811091460113517"
          },
          "Lo": {
            "Hi": "5134667498046831133",
            "Lo": "17102463161894732817"
          }
        },
        "index": "1"
      },
      {
        "address": {
          "Hi": {},
          "Lo": 4112
        },
        "topics": [
          {
            "Hi": {
              "Hi": "5619959878451166684",
              "Lo": "4467897518082841712"
            },
            "Lo": {
              "Hi": "14029876896021960261",
              "Lo": "13379578588044139875"
            }
          },
          {
            "Hi": {},
            "Lo": {
              "Lo": "4112"
            }
          },
          {
            "Hi": {
              "Lo": "3135307059"
            },
            "Lo": {
              "Hi": "10204824415584994316",
              "Lo": "3399674193463540339"
            }
          },
          {
            "Hi": {
              "Lo": "2128878986"
            },
            "Lo": {
              "Hi": "2694296070973497077",
              "Lo": "16622375008349742768"
            }
          }
        ],
        "data": "AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAANQ7SdvvspEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAD0Ecv0kyJ2mAAAAAAAAAAAAAAAAAAAAAAAAAAAAB3D3jO+tzNY//OmAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA8z2QqrcyxAcAAAAAAAAAAAAAAAAAAAAAAAAAAAAdw940kvJ9NO+mNw==",
        "blockNumber": "90386048",
        "txHash": {
          "Hi": {
            "Hi": "14862281939317677951",
            "Lo": "6963111081983623462"
          },
          "Lo": {
            "Hi": "14023204965242737735",
            "Lo": "58585669732858373"
          }
        },
        "blockHash": {
          "Hi": {
            "Hi": "3272624667754571407",
            "Lo": "4672811091460113517"
          },
          "Lo": {
            "Hi": "5134667498046831133",
            "Lo": "17102463161894732817"
          }
        },
        "index": "2"
      }
    ],
    "txHash": {
      "Hi": {
        "Hi": "14862281939317677951",
        "Lo": "6963111081983623462"
      },
      "Lo": {
        "Hi": "14023204965242737735",
        "Lo": "58585669732858373"
      }
    },
    "contractAddress": {
      "Hi": {}
    },
    "gasUsed": "105645",
    "effectiveGasPrice": "815144193098",
    "blockHash": {
      "Hi": {
        "Hi": "3272624667754571407",
        "Lo": "4672811091460113517"
      },
      "Lo": {
        "Hi": "5134667498046831133",
        "Lo": "17102463161894732817"
      }
    },
    "blockNumber": "90386048"
  }
}
```
{% endcode %}

{% hint style="info" %}
**H160 and H256 encoding.** In JSON representations of protobuf messages, `common.H160` (20-byte addresses) and `common.H256` (32-byte hashes) are shown here as hex strings for readability. The actual JSON wire format from grpcurl or `MessageToJson` reflects the message structure defined in [`common/common.proto`](https://github.com/0xPolygon/polyproto/blob/main/common/common.proto) — typically a nested object encoding. Applications using the typed generated clients receive `H160`/`H256` as strongly-typed message objects, not strings.
{% endhint %}

## Response Fields

| Field                       | Type         | Description                                                                     |
| --------------------------- | ------------ | ------------------------------------------------------------------------------- |
| `receipt`                   | Receipt      | The transaction receipt — see fields below                                      |
| `receipt.type`              | uint64       | Transaction type — `0` (legacy), `1` (EIP-2930), `2` (EIP-1559), `3` (EIP-4844) |
| `receipt.postState`         | bytes        | Post-state root (pre-Byzantium receipts only; empty on modern chains)           |
| `receipt.status`            | uint64       | `1` = success, `0` = reverted                                                   |
| `receipt.cumulativeGasUsed` | uint64       | Total gas consumed in the block up to and including this transaction            |
| `receipt.bloom`             | Bloom        | Bloom filter over this transaction's logs                                       |
| `receipt.logs`              | array of Log | Event logs emitted during execution                                             |
| `receipt.txHash`            | H256         | The transaction hash                                                            |
| `receipt.contractAddress`   | H160         | Address of newly deployed contract, or zero address for normal transactions     |
| `receipt.gasUsed`           | uint64       | Actual gas consumed by this transaction                                         |
| `receipt.effectiveGasPrice` | int64        | Effective gas price paid (for EIP-1559 accounting)                              |
| `receipt.blobGasUsed`       | uint64       | Blob gas consumed (EIP-4844)                                                    |
| `receipt.blobGasPrice`      | int64        | Blob gas price paid (EIP-4844)                                                  |
| `receipt.blockHash`         | H256         | Hash of the block that included this transaction                                |
| `receipt.blockNumber`       | int64        | Block number that included this transaction                                     |
| `receipt.transactionIndex`  | uint64       | Position of this transaction within its block                                   |

## Use Cases

* Confirming transaction success or failure after submission
* Reading event logs for indexers, wallets, and analytics
* Computing actual transaction cost (`gasUsed × effectiveGasPrice`)
* Detecting newly deployed contract addresses

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
| NOT\_FOUND (5)          | receipt not found                 | Transaction hasn't been mined yet, or doesn't exist                              |

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
    "hash": "0xa8f9b3c7d2e4f6a1b9c8d7e6f5a4b3c2d1e0f9a8b7c6d5e4f3a2b1c0d9e8f7a6"
};
const response = await new Promise((resolve, reject) => {
    client.TransactionReceipt(request, metadata, (err, res) => err ? reject(err) : resolve(res));
});
console.log(response);
```
{% endcode %}
{% endtab %}

{% tab title="Rust (tonic)" %}
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
use bor::ReceiptRequest;

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

    let request = tonic::Request::new(ReceiptRequest { /* fields */ });
    let response = client.transaction_receipt(request).await?;
    println!("{:?}", response);
    Ok(())
}
```
{% endtab %}
{% endtabs %}
