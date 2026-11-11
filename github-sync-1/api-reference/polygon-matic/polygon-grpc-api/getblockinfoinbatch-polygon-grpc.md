---
description: >-
  Example code for the GetBlockInfoInBatch grpc method. Сomplete guide on how to
  use GetBlockInfoInBatch grpc in GetBlock.io Web3 documentation.
---

# GetBlockInfoInBatch - Polygon gRPC

Returns headers, total difficulty, and block author addresses for every block in a range `[startBlockNumber, endBlockNumber]` — in a single call. This is the flagship method that gives Heimdall its \~4.4× milestone proposition speedup over the equivalent HTTP JSON-RPC path (which required N separate calls per block). When proposing a milestone covering \~22 blocks, this replaces 22 separate `HeaderByNumber` + 22 `GetAuthor` + 22 `GetTdByNumber` calls with one batched round trip. Essential for any application that needs bulk block metadata for a range.

## Request Parameters

| Parameter          | Type   | Required | Description                          |
| ------------------ | ------ | -------- | ------------------------------------ |
| `startBlockNumber` | uint64 | Yes      | First block in the range (inclusive) |
| `endBlockNumber`   | uint64 | Yes      | Last block in the range (inclusive)  |

## Request Example

{% tabs %}
{% tab title="grpcurl" %}
```bash
grpcurl -H 'x-access-token: <ACCESS-TOKEN>' \
        -d '{"startBlockNumber": "90385120", "endBlockNumber": "90385122"}' \
        shared.ap-southeast-1.getblock.io:443 \
        bor.BorApi/GetBlockInfoInBatch
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
    "startBlockNumber": "90385120",
    "endBlockNumber": "90385122"
};

client.GetBlockInfoInBatch(request, metadata, (err, response) => {
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

request = bor_pb2.GetBlockInfoInBatchRequest(
    startBlockNumber="90385120",
    endBlockNumber="90385122",
)
response = stub.GetBlockInfoInBatch(request, metadata=metadata)

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

    req := &bor.GetBlockInfoInBatchRequest{
        StartBlockNumber: "90385120",
        EndBlockNumber: "90385122",
    }
    resp, err := client.GetBlockInfoInBatch(ctx, req)
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
  "blocks": [
    {
      "header": {
        "number": "90385120",
        "parentHash": {
          "Hi": {
            "Hi": "7404332377439225495",
            "Lo": "15785647795632694924"
          },
          "Lo": {
            "Hi": "10455315323507691763",
            "Lo": "13030904670670349618"
          }
        },
        "time": "1784282677",
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
            "Hi": "8705662766577384732",
            "Lo": "8650090463967266327"
          },
          "Lo": {
            "Hi": "4841782280800027016",
            "Lo": "12887489940187659777"
          }
        },
        "txRoot": {
          "Hi": {
            "Hi": "4911347614093142073",
            "Lo": "9480473189619841625"
          },
          "Lo": {
            "Hi": "9908053757987450028",
            "Lo": "1652020289865110993"
          }
        },
        "receiptRoot": {
          "Hi": {
            "Hi": "2514713256969833212",
            "Lo": "15573225728988374643"
          },
          "Lo": {
            "Hi": "13791318704405182634",
            "Lo": "8500567600303507877"
          }
        },
        "bloom": "H+VWNkMH1sKJPGm3tV5IxTP9StrYgT1zK4iKZid64ip9wHZBKnfLVnkfH7TiyX1DoYifPF7ieM9k0cCnNDVgIPWU374zK8pes7yDS9gjtqC99cx3sVSdOXfvjVWjhI/X9zCnGN/lqYtTG59fjcKJrr7DM/0u3VS9rITK9r6Zrv/V1T3VfKjrIqc9z/s015AXdT73hbdjoGnyOYVAjaB6GXM5QId+rK3JZBoqblnQ7YdcXejjE7QkSWcOdusorKnt7JK3tmtPt0/gA22IWc4YbWZcn2fVjLBZM3apUgDk+bZiUKvpHbnJm+VFhdvaLLc9pWEmLB5wEcH/CBudMhXrww==",
        "difficulty": "AQ==",
        "gasLimit": "160000000",
        "gasUsed": "42837853",
        "extraData": "14MBEAiDYm9yiGdvMS4yNi4zhWxpbnV4AAAAAAAAAAD45oD43cDAwMDBAsEEwQXBBcIGB8DBCcEIwQjBDMILDcDBDsEQwRHBEsETwRTBFcEWwRfBGMEZwRrAwRzCgB3AwQnBGsEhwMEiwMIKGcDCIhvBKMIkKcEqwMErwS3BJsEmwSDCLDDBMsEzwTTBNcE2wTfBOME5wTrBO8E8wT3BPsE/wUDBQcFCwUPBRMFFwUbBR8FIwUnBSsFLwUzBTcFOwU/BUMFRwVLBU8FUwVXBVsFXwVjBWcFawVvBW8FcwMFfwS/BLsFiwWPBZMDAwMExwMFlwWvAwWzBbsFvwMFwwMFyhAFuNgBACROKfcRHWlMzlCkg3MYfgd0g8pf8vlOc4UiaAERR3nklZKvYSnK4mtcboallx7r7Mu0QLAXYN1JjNS5AXJ06FgE=",
        "mixDigest": {
          "Hi": {},
          "Lo": {}
        },
        "nonce": "AAAAAAAAAAA=",
        "baseFee": "Oe/P4J0="
      },
      "totalDifficulty": "1506972013",
      "author": {
        "Hi": {
          "Hi": "2718481128240167593",
          "Lo": "13848821177151473057"
        },
        "Lo": 1989474573
      }
    },
    {
      "header": {
        "number": "90385121",
        "parentHash": {
          "Hi": {
            "Hi": "12467844754646264338",
            "Lo": "13344664344561581192"
          },
          "Lo": {
            "Hi": "14707377003300390153",
            "Lo": "586774422848354107"
          }
        },
        "time": "1784282679",
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
            "Hi": "7282806575262382277",
            "Lo": "2828681249388101166"
          },
          "Lo": {
            "Hi": "8285571706407203631",
            "Lo": "1393162292370197811"
          }
        },
        "txRoot": {
          "Hi": {
            "Hi": "3377307875058571836",
            "Lo": "17675710139042458362"
          },
          "Lo": {
            "Hi": "10309909134800761891",
            "Lo": "15305919657482738658"
          }
        },
        "receiptRoot": {
          "Hi": {
            "Hi": "1918743066857010846",
            "Lo": "542664586653111184"
          },
          "Lo": {
            "Hi": "895449310215360758",
            "Lo": "9662677478596186794"
          }
        },
        "bloom": "3rFCP2bDNK9qnr0Whkvkj0FgTCBaFZkuaIXGKSzUzihFCJYVN9rfEY3/Hv3x2R1JGKzJrBLnqHbtEBu3VHBh4G+VkPIXJOIHoOhXbLg+lqYmPiSvC1jQOUEz2RHhMqvD1wDKUIbZmypFK2OjrciL1EYAY/V9Olm1uARLNqIBrbFD0dsVlcgFELbCny02xfCYyVWNswZS8t7RkWDKyQr72abQ4MxXlIfDh1PqJShFvWxZz0gVqYS3QXdCRh6an83nxGz8U5vXouNgiWuoWeKSlednHOLuj/AVkFu4PTtldQBqum/4/rgEpSUDUBAkB9VBvCcTPxzwycWQwEiNehv5QQ==",
        "difficulty": "AQ==",
        "gasLimit": "160000000",
        "gasUsed": "30535975",
        "extraData": "14MBEAiDYm9yiGdvMS4yNi4zhWxpbnV4AAAAAAAAAAD4xoD4vcDAwMECwMECwgMFwQbAwQfBCcEKwQvBDMENwQ7BD8EQwRHBEsETwRTBFcEWwRfBCMEZwQjAwgQUwR3CGB3AwMEfwSLBI8EkwMElwhsnwSjBGsEqwSnBLMEtwS7BL8EwwTHBMsEzwTTBNcE2wTfBOME5wTrBO8E8wT3BPsE/wUDBQcFCwUPBRMFFwMFHwitGwSjBSsDAwUvBTsDBT8FRwUXBU8FUwVXBVsFXwVjCHlHCgFLAwMJJWcFbwV7BXoQBbjYAQFYYIic+dWMAxtXg8+4xS/TklEsy1FVK3Qt+aljrbe18Ua07vSnLBFmZJOsNfgk3sPSOf6ap6EeTnN04ga+8DTEA",
        "mixDigest": {
          "Hi": {},
          "Lo": {}
        },
        "nonce": "AAAAAAAAAAA=",
        "baseFee": "OqW2dbc="
      },
      "totalDifficulty": "1506972014",
      "author": {
        "Hi": {
          "Hi": "2718481128240167593",
          "Lo": "13848821177151473057"
        },
        "Lo": 1989474573
      }
    },
    {
      "header": {
        "number": "90385122",
        "parentHash": {
          "Hi": {
            "Hi": "16871868229211049473",
            "Lo": "11336726918917237151"
          },
          "Lo": {
            "Hi": "3489773670040155428",
            "Lo": "8927960470683782187"
          }
        },
        "time": "1784282680",
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
            "Hi": "12601868136448473051",
            "Lo": "3690116273571716268"
          },
          "Lo": {
            "Hi": "15274699286913162378",
            "Lo": "3571074499290086562"
          }
        },
        "txRoot": {
          "Hi": {
            "Hi": "18067367849311534222",
            "Lo": "6544063175066280127"
          },
          "Lo": {
            "Hi": "16604868534174255832",
            "Lo": "5134743783065360427"
          }
        },
        "receiptRoot": {
          "Hi": {
            "Hi": "14322219054689647888",
            "Lo": "15478667276447832241"
          },
          "Lo": {
            "Hi": "11564071099287805630",
            "Lo": "8868207404455397871"
          }
        },
        "bloom": "H2HMP2PCofHYuDeSpWLoEEJwXDDZ9agoP4YfopVWaOJ9SxYhJkzIEilXBrkgURtLfUHn7Gxn50RsxOK3ZvYoYNVctnRSruLMoLI0zDI7o/sNoYQsrdcjEFP7oj0pAoLJ9oIIUQMg2OzKi1AzFem+4c+ko9Fq3iaz+JlC90PeZeDF0a2wVrhBdacbn+yWQNAU6ROXoTZX8lzEI2hCETX4g6qdlZA/lrHBVdYaILHBz5JN/kjEEZuieWFiUpMTBW7vojp0V1/CoGFRJVngWsZRV0FVRfDEH7qVe1iZmgrPOctilHbMVPryoaUDtVPEprl5JrYiRDRFy9E3QnzVnnh5xw==",
        "difficulty": "AQ==",
        "gasLimit": "160000000",
        "gasUsed": "33459430",
        "extraData": "14MBEAiDYm9yiGdvMS4yNi4zhWxpbnV4AAAAAAAAAAD42YD40MDAwQHBAsEDwQTBBcEGwQfAwQjAwMEKwQ3BDsEPwRDBEcESwRPBFMEVwRbBF8EYwRnBGsEbwRzAwMIJHcDBHcIgIsEjwMEkwSbBC8EowSnBC8ErwioswS3BLsEvwTDBMcEywTPBNME1wTbBN8E4wTnBOsE7wTzBPcE+wT/BQMFAwULCQUPBRMFFwUbBR8FIwUnBSsFLwUzBTcFOwU/BUMFRwVLBT8FTwVXAwTXBJsEnwVnAwVjBXcDBX8FgwVbBW8FjwWTBZcFmwSDAwWLBasCEBqz8AEBTEbAsu/KxDld64AC50BFyCyAtr98mf5A+lkWWAgrEemQEh2ULwiE8D5dd6ZN0Fw4DXOQg6ddUPaJnZp91VMBJAQ==",
        "mixDigest": {
          "Hi": {},
          "Lo": {}
        },
        "nonce": "AAAAAAAAAAA=",
        "baseFee": "OfsVHyQ="
      },
      "totalDifficulty": "1506972015",
      "author": {
        "Hi": {
          "Hi": "2718481128240167593",
          "Lo": "13848821177151473057"
        },
        "Lo": 1989474573
      }
    }
  ]
}
```
{% endcode %}

{% hint style="info" %}
**H160 and H256 encoding.** In JSON representations of protobuf messages, `common.H160` (20-byte addresses) and `common.H256` (32-byte hashes) are shown here as hex strings for readability. The actual JSON wire format from grpcurl or `MessageToJson` reflects the message structure defined in [`common/common.proto`](https://github.com/0xPolygon/polyproto/blob/main/common/common.proto) — typically a nested object encoding. Applications using the typed generated clients receive `H160`/`H256` as strongly-typed message objects, not strings.
{% endhint %}

## Response Fields

| Field                      | Type               | Description                                               |
| -------------------------- | ------------------ | --------------------------------------------------------- |
| `blocks`                   | array of BlockInfo | Per-block info records, one per block in the range        |
| `blocks[].header`          | Header             | Full block header — same shape as `HeaderByNumber.header` |
| `blocks[].totalDifficulty` | uint64             | Cumulative TD at this block                               |
| `blocks[].author`          | H160               | Address of the block producer                             |

## Use Cases

* Heimdall milestone proposition — bulk fetch of block metadata for the milestone range (the primary consumer; \~4.4× speedup over HTTP)
* Bulk backfill for block indexers when catching up a range
* Producer distribution analytics over a block range
* Rebuilding a chain-verification snapshot for audit tooling

## Error Handling

| gRPC Status (Code)      | Message                           | Cause                                                                                                         |
| ----------------------- | --------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| UNAUTHENTICATED (16)    | missing or invalid x-access-token | Access token missing from `x-access-token` gRPC metadata or invalid                                           |
| PERMISSION\_DENIED (7)  | access denied                     | Access token doesn't have permission for this method or endpoint                                              |
| INVALID\_ARGUMENT (3)   | invalid request                   | Request message is malformed or fails validation (e.g. invalid block number tag)                              |
| INTERNAL (13)           | internal error                    | Server-side error while processing the request                                                                |
| UNAVAILABLE (14)        | service unavailable               | Bor node is temporarily unavailable (syncing, restart, or overload)                                           |
| DEADLINE\_EXCEEDED (4)  | deadline exceeded                 | Request took longer than the client-configured timeout                                                        |
| RESOURCE\_EXHAUSTED (8) | rate limit exceeded               | Rate limit exceeded for your plan                                                                             |
| INVALID\_ARGUMENT (3)   | invalid block range               | `endBlockNumber` less than `startBlockNumber`, or range exceeds implementation limits (\~1000 blocks typical) |
| NOT\_FOUND (5)          | block range not available         | One or both endpoint blocks are beyond the current chain tip                                                  |
| RESOURCE\_EXHAUSTED (8) | range too large                   | Requested range exceeds the maximum blocks per call permitted by this endpoint                                |

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
    "startBlockNumber": "90385120",
    "endBlockNumber": "90385122"
};
const response = await new Promise((resolve, reject) => {
    client.GetBlockInfoInBatch(request, metadata, (err, res) => err ? reject(err) : resolve(res));
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
use bor::GetBlockInfoInBatchRequest;

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

    const request = tonic::Request::new(GetBlockInfoInBatchRequest { /* fields */ });
    let response = client.get_block_info_in_batch(request).await?;
    println!("{:?}", response);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}
