---
description: >-
  GetBlock provides fast and reliable access to Polygon nodes via gRPC API.
  Connect to the Polygon network without running your own infrastructure.
---

# Polygon gRPC API

Polygon gRPC is the **binary Protocol Buffers interface** exposed by Bor v2.8.3+ for high-performance consensus-layer coordination with Heimdall v2. It is not a replacement for Bor's Ethereum JSON-RPC — the `eth_*` execution surface remains the primary developer interface for smart contracts, transactions, and account state.

What Bor gRPC provides is a **strongly typed, HTTP/2 Multiplexed transport for the specific set of calls that** Heimdall v2 makes against Bor to build checkpoints, propose milestones, verify block producers, and correlate spans. Because it uses Protocol Buffers instead of JSON-RPC, milestone proposition — the single hottest path in the Heimdall→Bor interaction — runs approximately 4.4× faster than the equivalent HTTP JSON-RPC round trip (per Polygon Labs' devnet benchmarks).

The service is defined at [`https://github.com/0xPolygon/polyproto/tree/main/bor`](https://github.com/0xPolygon/polyproto/tree/main/bor) and exposes a **single gRPC service — `bor.BorApi` — with 11 unary methods**. There are no streaming endpoints and no bidirectional calls; every method is a request/response pair. Bor v2.8.3 introduces the server implementation (disabled by default, loopback bind on activation, PR #2194); Heimdall v2 v0.9.0 introduces the client side (opt-in via `bor_grpc_flag = "true"` in `app.toml`). HTTP JSON-RPC remains the default transport for backward compatibility.

### Key Features

* **Single Service, 11 Methods**: `bor.BorApi` — no service sprawl, no versioned aliases, no deprecated legacy names. Every method has a purpose in the Heimdall↔Bor coordination path
* **Protocol Buffers Over HTTP/2**: strongly-typed schemas via the polyproto `.proto` files; binary encoding produces \~3-5× smaller payloads than JSON-RPC and enables HTTP/2 multiplexing
* **Batched Block Info in One Round Trip**: `GetBlockInfoInBatch` returns headers + total difficulty + author addresses for `[startBlockNumber, endBlockNumber]` — replacing what used to require N separate JSON-RPC calls per block with a single gRPC call
* **Native Bor System Receipts**: `BorBlockReceipt` exposes the sprint-boundary state-sync transaction receipts that Bor generates internally — a Bor-native concept with no direct Ethereum equivalent, essential for bridge indexers tracking Ethereum → Polygon state sync flow
* **Consensus-Grade Root Hashes**: `GetRootHash(startBlock, endBlock)` computes the Merkle root over a Bor block range in the exact form Heimdall uses to build checkpoint submissions to Ethereum L1
* **Milestone Voting Support**: `GetVoteOnHash` accepts a milestone ID and hash, and returns whether the local Bor node accepts the milestone proposal — the primary voting input for validator milestone acknowledgment
* **Bearer Token Authentication**: gRPC metadata carries an authorization token. Loopback deployments can leave it empty (Bor's `[grpc]` section defaults to loopback bind); public exposure requires the token to be set
* **Zero Impact on Ethereum JSON-RPC**: gRPC and JSON-RPC coexist on the same Bor node. Enabling gRPC doesn't change `eth_*` method behavior. Applications using Bor for standard EVM reads/writes need not migrate

{% hint style="info" %}
_**TECHNICAL DISCLAIMER: AUTHORITATIVE BOR GRPC SPECIFICATION**_

_GetBlock's Polygon Bor gRPC API reference documentation is provided exclusively for informational purposes and to optimize the developer experience. The canonical and normative specifications for the `bor.BorApi` service — including exact protobuf message schemas, field ordering, and the shared `common.H160` / `common.H256` type definitions — are maintained by:_

* _**Polygon**:_ [_github.com/0xPolygon/polyproto_](https://github.com/0xPolygon/polyproto) _— the `.proto` files are the source of truth. `bor/bor.proto` defines this service; `common/common.proto` defines shared `H160` (20-byte address) and `H256` (32-byte hash) types_
* _**Bor**:_ [_github.com/0xPolygon/bor_](https://github.com/0xPolygon/bor) _— the reference server implementation (see PR #2194 and the_ [_`internal/cli/server/api_service.go`_](https://github.com/0xPolygon/bor/blob/master/internal/cli/server/api_service.go) _file for the Go-side implementation)_
* _**Heimdall v2**:_ [_github.com/0xPolygon/heimdall-v2_](https://github.com/0xPolygon/heimdall-v2) _— the reference client, and the primary consumer for which this API was designed_
{% endhint %}

### Network Information

| Property                 | Value                                                                         |
| ------------------------ | ----------------------------------------------------------------------------- |
| Service                  | `bor.BorApi`                                                                  |
| Proto Package Path       | `bor` (defined in `polyproto/bor/bor.proto`)                                  |
| Total Methods            | 11 (all unary — no streaming)                                                 |
| Common Types             | `common.H160` (20-byte address), `common.H256` (32-byte hash)                 |
| Protocol                 | Protocol Buffers over HTTP/2 (TLS on public endpoints)                        |
| Authentication           | Bearer token in gRPC metadata (`authorization: Bearer <TOKEN>`)               |
| Default Self-Hosted Port | 3131 (loopback bind by default in Bor v2.8.3+)                                |
| Server Version Required  | Bor v2.8.3 or newer                                                           |
| Reference Client Version | Heimdall v2 v0.9.0 or newer                                                   |
| Bor Config Section       | `[grpc]` in `config.toml` — `addr = "127.0.0.1:3131"`, `token = ""`           |
| Server Reflection        | Depends on Bor build (verify with `grpcurl list` against production endpoint) |
| Bor Chain ID (Mainnet)   | 137                                                                           |

### Base URL

Polygon Bor gRPC is exposed on GetBlock's standard TLS endpoint pattern — the same pattern used by TRON gRPC and Solana Yellowstone gRPC. Select the region closest to your consumer when creating the endpoint in the GetBlock dashboard.

{% tabs %}
{% tab title="Frankfurt, Germany" %}
```bash
https://shared.eu-central-1.getblock.io/
```
{% endtab %}

{% tab title="New York, USA" %}
```bash
https://shared.us-east-1.getblock.io/
```
{% endtab %}

{% tab title="Singapore, Singapore" %}
```bash
https://shared.ap-southeast-1.getblock.io/
```
{% endtab %}
{% endtabs %}

### Supported Networks

| Network                       | gRPC | Frankfurt | New York | Singapore |
| ----------------------------- | ---- | --------- | -------- | --------- |
| Polygon PoS Mainnet (Bor 137) | ✅    | ✅         | ✅        | ✅         |

### Quickstart

In this section, you will learn how to fetch the **latest Bor block header** — the most recently produced block on Polygon PoS — using either:

* **`grpcurl`** (CLI — the universal gRPC probe tool)
* **JavaScript** (Node.js with `@grpc/grpc-js`)
* **Python** (with `grpcio`)

`HeaderByNumber` is a good first call because it uses the same "block number or tag" convention (`"latest"`, `"earliest"`, `"pending"`, `"finalized"`, `"safe"`, or hex `"0x..."`) as standard Ethereum JSON-RPC, so its input semantics are immediately familiar. Its response validates two things at once: your gRPC connection works, and the response schema matches the `polyproto/bor/bor.proto` `Header` message.

{% tabs %}
{% tab title="grpcurl (CLI)" %}
{% stepper %}
{% step %}
#### Install grpcurl

`grpcurl` is the universal gRPC exploration tool. It's like `curl` for gRPC — no `.proto` files required if the server supports gRPC reflection; otherwise pass `.proto` files via `-proto`.

{% code overflow="wrap" %}
```bash
# macOS
brew install grpcurl

# Linux (from GitHub releases)
curl -LO https://github.com/fullstorydev/grpcurl/releases/latest/download/grpcurl_linux_x86_64.tar.gz
tar xzf grpcurl_linux_x86_64.tar.gz
sudo mv grpcurl /usr/local/bin/

# Windows (via Scoop)
scoop install grpcurl
```
{% endcode %}
{% endstep %}

{% step %}
### Vendor the polyproto files&#x20;

GetBlock's endpoint has gRPC server reflection enabled, `grpcurl list` which shows every method automatically, and you can skip this step if you prefer to clone the proto files directly:

```bash
git clone https://github.com/0xPolygon/polyproto.git
```

Then pass them to `grpcurl` via `-import-path` and `-proto`.
{% endstep %}

{% step %}
### Call `HeaderByNumber` with `"latest"`

With reflection enabled:

{% code overflow="wrap" %}
```bash
grpcurl -H 'x-access-token: <ACCESS_TOKEN>' \
        -d '{"number": "latest"}' \
        shared.ap-southeast-1.getblock.io:443\
        bor.BorApi/HeaderByNumber
```
{% endcode %}

Without reflection (using local proto files):

{% code overflow="wrap" %}
```bash
grpcurl -import-path ./polyproto \
        -proto bor/bor.proto \
        -H 'x-access-token: <ACCESS_TOKEN>' \
        -d '{"number": "latest"}' \
        shared.us-east-1.getblock.io:443\
        bor.BorApi/HeaderByNumber
```
{% endcode %}

Replace `<ACCESS-TOKEN>` with your actual GetBlock access token.
{% endstep %}

{% step %}
### Expected output

{% code title="" overflow="wrap" %}
```bash
{
  "header": {
    "number": "90383503",
    "parentHash": {
      "Hi": {
        "Hi": "8001495684790200550",
        "Lo": "12820516500619814269"
      },
      "Lo": {
        "Hi": "12820709937774159835",
        "Lo": "10590621424795036054"
      }
    },
    "time": "1784280252",
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
        "Hi": "3861539587187450034",
        "Lo": "7546199042364005968"
      },
      "Lo": {
        "Hi": "249015285448705997",
        "Lo": "16479773740607020354"
      }
    },
    "txRoot": {
      "Hi": {
        "Hi": "12980661894323543903",
        "Lo": "1577448976556285933"
      },
      "Lo": {
        "Hi": "6785559408085401529",
        "Lo": "5091134292871561698"
      }
    },
    "receiptRoot": {
      "Hi": {
        "Hi": "3131572995761028724",
        "Lo": "12231827564059625448"
      },
      "Lo": {
        "Hi": "1608246602846981995",
        "Lo": "16695883862857860181"
      }
    },
    "bloom": "fmHEfsIdurzMvT0gtg/6wWE2T+b/BamCoo2TbD50YLzGRdbN78RexMGd0vdoyyV1c+i49gz7uG/jfpiFozZ/968WpRO+bfKKKakoX5CinbIkprwIC/9MFdHjNJuYZ+uPx+6A8n8ZnC5rT+vXpcSNK+4Pu9NreF5X+hrXmEPIraGb6QtRQcx7Vee6rfYU4p+RRhuP7QbVHl9wX9lwhx2yGLKLV7DWemu0BpsutPRUsx9qb1XxOa0uSV+pcSJPr9nd4Nq393/GrsvSYc/KWcwRbWTED+PlD6eVEXe4vwB+ctBn+m/fPFgGGRU3BXMvMJHRvN1SRThgO3AUCg0n0hnrDw==",
    "difficulty": "AQ==",
    "gasLimit": "160000000",
    "gasUsed": "36671898",
    "extraData": "14MBEAiDYm9yiGdvMS4yNi4zhWxpbnV4AAAAAAAAAAD49YD47MDBgMDAwoADwQTAwQXAwMEHwQrBC8EMwQ3BDsICDsEQwRHBEsIPE8EUwRXBFsEXwRjBGcEawRvAwMDBHMEgwSHBIsESwiMkwSXBJsEnwSjBKcEqwQHBLMDBHsEvwTDBMcEywTPBNMDAwSvBOMDBOcEtwTvBO8E+wT/CPUDBQcFCwUPBRMDBPMIeR8FIwkk1wUrBS8FMwU3BTsFPwVDBUcFRwVPBVMJSVcFWwVfBWMFZwVrBW8FcwV3BXsFfwWDBYcFiwWPBZMDAwTrAwMFqwMFFwW3AwQHAwMFuwXPCZXTAwWXCOnXBcMDBesF7hAas/ABAsnoC4VUmV488IhxyqA3HCZhw8uGdCrBauiFzkd8EPpduPmuBL8FR8h5mc3E6IqK8F/QOfQ37WwP7sOk3sFOE8gE=",
    "mixDigest": {
      "Hi": {},
      "Lo": {}
    },
    "nonce": "AAAAAAAAAAA=",
    "baseFee": "OcXBxBQ="
  }
}
```
{% endcode %}

This contain `GetHeaderByNumberResponse` message containing a `Header` object with the current block number, parent hash, timestamp, gas limit, and all other standard Ethereum header fields. The `number` field will be the height of the block Bor considers `latest` at the moment of the call.
{% endstep %}
{% endstepper %}
{% endtab %}

{% tab title="JavaScript (Node.js)" %}
{% stepper %}
{% step %}
### Setup project

```bash
mkdir polygon-bor-quickstart
cd polygon-bor-quickstart
npm init --yes
```
{% endstep %}

{% step %}
### Install gRPC dependencies

```bash
npm install @grpc/grpc-js @grpc/proto-loader
```
{% endstep %}

{% step %}
### Vendor the polyproto files

{% code overflow="wrap" %}
```bash
git clone https://github.com/0xPolygon/polyproto.git protos
```
{% endcode %}

The Bor proto imports `common/common.proto` — cloning the whole repo makes sure both are present.
{% endstep %}

{% step %}
### Set ES module type

Add `"type": "module"` to your `package.json`.
{% endstep %}

{% step %}
### Add code

{% code title="index.js" overflow="wrap" %}
```javascript
import * as grpc from '@grpc/grpc-js';
import protoLoader from '@grpc/proto-loader';

const PROTO_PATH = './protos/bor/bor.proto';
const ENDPOINT = 'shared.ap-southeast-1.getblock.io:443';
const ACCESS_TOKEN = '<ACCESS-TOKEN>';

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

const client = new BorApiClient(ENDPOINT, grpc.credentials.createSsl());
const metadata = new grpc.Metadata();
metadata.add('x-access-token', `${ACCESS_TOKEN}`);

client.HeaderByNumber({ number: 'latest' }, metadata, (err, response) => {
    if (err) {
        console.error('gRPC error:', err.code, err.message);
        return;
    }
    console.log(JSON.stringify(response, null, 2));
});
```
{% endcode %}

Replace `<ACCESS-TOKEN>` with your actual GetBlock access token.
{% endstep %}

{% step %}
### Run the script

```bash
node index.js
```

The response contains the latest Bor block's header.

{% code title="latest Bor block's header." overflow="wrap" %}
```bash
{
  "header": {
    "number": "90383984",
    "parentHash": {
      "Hi": {
        "Hi": "3219579091769836205",
        "Lo": "3715250197314926109"
      },
      "Lo": {
        "Hi": "2674255279228011676",
        "Lo": "3095247116736800573"
      }
    },
    "time": "1784280973",
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
      "Hi": {
        "Hi": "0",
        "Lo": "0"
      },
      "Lo": 0
    },
    "stateRoot": {
      "Hi": {
        "Hi": "9833850037642172310",
        "Lo": "14374670924963563757"
      },
      "Lo": {
        "Hi": "15326292854892058618",
        "Lo": "3327665482045070470"
      }
    },
    "txRoot": {
      "Hi": {
        "Hi": "1622856331791778660",
        "Lo": "17858003608247780972"
      },
      "Lo": {
        "Hi": "11571743316523524888",
        "Lo": "18243495908713277801"
      }
    },
    "receiptRoot": {
      "Hi": {
        "Hi": "3315218599047333559",
        "Lo": "8266824338816502615"
      },
      "Lo": {
        "Hi": "17685214896531518585",
        "Lo": "53913688016064401"
      }
    },
    "bloom": {
      "type": "Buffer",
      "data": [
        255,
        255,
        255,
        255,
        247,
        255,
        255,
        255,
        255
      ]
    },
    "difficulty": {
      "type": "Buffer",
      "data": [
        1
      ]
    },
    "gasLimit": "160000000",
    "gasUsed": "52014949",
    "extraData": {
      "type": "Buffer",
      "data": [
       
        35,
        192,
        193,
        36,
        193,
        38,
        193,
        88,
        193,
        89,
        193,
        90,
        193,
        91,
        193,
        0,
        64,
        128,
        174,
        1
      ]
    },
    "mixDigest": {
      "Hi": {
        "Hi": "0",
        "Lo": "0"
      },
      "Lo": {
        "Hi": "0",
        "Lo": "0"
      }
    },
    "nonce": {
      "type": "Buffer",
      "data": [
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0
      ]
    },
    "baseFee": {
      "type": "Buffer",
      "data": [
        57,
        150,
        194,
        243,
        88
      ]
    },
    "withdrawalsHash": null,
    "parentBeaconBlockRoot": null,
    "requestsHash": null
  }
}
```
{% endcode %}
{% endstep %}
{% endstepper %}
{% endtab %}

{% tab title="Python" %}
{% stepper %}
{% step %}
### Setup project

```bash
mkdir polygon-bor-quickstart
cd polygon-bor-quickstart
```
{% endstep %}

{% step %}
### Create and activate a virtual environment

```bash
python3 -m venv venv
source venv/bin/activate

# On Windows, 
use venv\Scripts\activate
```
{% endstep %}

{% step %}
### Install gRPC dependencies

```bash
pip install grpcio grpcio-tools
```
{% endstep %}

{% step %}
### Vendor and compile the proto files

```bash
git clone https://github.com/0xPolygon/polyproto.git protos

python -m grpc_tools.protoc \
    -I protos \
    --python_out=. \
    --grpc_python_out=. \
    protos/bor/bor.proto \
    protos/common/common.proto
```

This generates `bor/bor_pb2.py`, `bor/bor_pb2_grpc.py`, and `common/common_pb2.py`.
{% endstep %}

{% step %}
### Create script

{% code title="main.py" %}
```python
import grpc
from google.protobuf.json_format import MessageToJson

from bor import bor_pb2, bor_pb2_grpc

ENDPOINT = "shared.ap-southeast-1.getblock.io:443"
ACCESS_TOKEN = "<ACCESS_TOKEN>"

credentials = grpc.ssl_channel_credentials()
channel = grpc.secure_channel(ENDPOINT, credentials)
stub = bor_pb2_grpc.BorApiStub(channel)

metadata = [("x-access-token", f" {ACCESS_TOKEN}")]

request = bor_pb2.GetHeaderByNumberRequest(number="latest")
response = stub.HeaderByNumber(request, metadata=metadata)

print(MessageToJson(response, indent=2))
```
{% endcode %}

Replace `<ACCESS-TOKEN>` with your actual access token from GetBlock.
{% endstep %}

{% step %}
### Run the script

```bash
python main.py
```
{% endstep %}
{% endstepper %}
{% endtab %}
{% endtabs %}

### Available API Methods

`bor.BorApi` exposes 11 unary methods. Organized by function:

#### Chain State Reads (3 methods)

| Method         | Description                                                               |
| -------------- | ------------------------------------------------------------------------- |
| HeaderByNumber | Returns a block header for a given block number or tag                    |
| BlockByNumber  | Returns a block (currently header-only per the schema) for a given number |
| GetAuthor      | Returns the block producer (author) address for a given block number      |

#### Receipt Reads (2 methods)

| Method             | Description                                                                               |
| ------------------ | ----------------------------------------------------------------------------------------- |
| TransactionReceipt | Returns the receipt for a transaction hash                                                |
| BorBlockReceipt    | Returns the Bor system-level block receipt (state-sync transactions at sprint boundaries) |

#### Fork-Choice Data (2 methods)

| Method        | Description                                     |
| ------------- | ----------------------------------------------- |
| GetTdByHash   | Returns total difficulty by block hash          |
| GetTdByNumber | Returns total difficulty by block number or tag |

#### Consensus Operations (2 methods)

| Method        | Description                                                                   |
| ------------- | ----------------------------------------------------------------------------- |
| GetRootHash   | Computes the Merkle root of a Bor block range for checkpoint construction     |
| GetVoteOnHash | Returns whether the local node accepts a milestone proposal for a block range |

#### Span Correlation (2 methods)

| Method                      | Description                                                                                                          |
| --------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| GetStartBlockHeimdallSpanID | Maps a Bor start block to its corresponding Heimdall span ID                                                         |
| GetBlockInfoInBatch         | Returns headers + total difficulty + author for a block range in one call — the \~4.4× milestone proposition speedup |

### Support

For technical support and questions:

* **Support Email**: [support@getblock.io](mailto:support@getblock.io)

### See Also

* [polyproto GitHub](https://github.com/0xPolygon/polyproto) — the source of truth for `bor.BorApi` and common types
* [`bor/bor.proto`](https://github.com/0xPolygon/polyproto/blob/main/bor/bor.proto) — the `bor.BorApi` service definition
* [`common/common.proto`](https://github.com/0xPolygon/polyproto/blob/main/common/common.proto) — the `H160` and `H256` message types used throughout
* [Bor v2.8.3 Release Notes](https://github.com/0xPolygon/bor/releases/tag/v2.8.3) — server implementation (PR #2194)
* [Polygon JSON-RPC](../)  — the standard EVM `eth_*` interface (separate from this gRPC service)
* [grpcurl Documentation](https://github.com/fullstorydev/grpcurl) — universal gRPC exploration tool used in the Quickstart
