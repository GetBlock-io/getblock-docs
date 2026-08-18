---
description: >-
  Example code for the GetBlockByLimitNext2 gRPC method. Complete guide on how
  to use GetBlockByLimitNext2 gRPC method in GetBlock Web3 documentation.
---

# GetBlockByLimitNext2 - Tron

This method returns a range of blocks between a start height (inclusive) and an end height (exclusive) in one call.

## Service

This method is served by the `protocol.Wallet` service.

## Method

```protobuf
rpc GetBlockByLimitNext2 (BlockLimit) returns (BlockListExtention)
```

## Request Message

`BlockLimit`

| Field    | Type  | Description                                |
| -------- | ----- | ------------------------------------------ |
| startNum | int64 | First block height in the range, inclusive |
| endNum   | int64 | End block height, exclusive                |

## Request

{% tabs %}
{% tab title="grpcurl" %}
{% code overflow="wrap" %}
```bash
grpcurl -H 'x-api-key: <ACCESS-TOKEN>' \
  -d '{"startNum": 68000000, "endNum": 68000005}' \
  shared.eu-central-1.getblock.io:443 protocol.Wallet/GetBlockByLimitNext2
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import grpc
# Stubs generated from java-tron's api.proto with grpcio-tools
from api import api_pb2_grpc
from core import Contract_pb2, Tron_pb2

creds = grpc.ssl_channel_credentials()
channel = grpc.secure_channel('shared.eu-central-1.getblock.io:443', creds)
metadata = [('x-api-key', '<ACCESS-TOKEN>')]
stub = api_pb2_grpc.WalletStub(channel)

response = stub.GetBlockByLimitNext2(api_pb2.BlockLimit(startNum=68000000, endNum=68000005), metadata=metadata)
print(response)
```
{% endcode %}
{% endtab %}

{% tab title="Go" %}
{% code title="example.go" %}
```go
conn, _ := grpc.Dial(
    "shared.eu-central-1.getblock.io:443",
    grpc.WithTransportCredentials(credentials.NewTLS(&tls.Config{})),
)
defer conn.Close()

client := api.NewWalletClient(conn)
ctx := metadata.AppendToOutgoingContext(
    context.Background(), "x-api-key", "<ACCESS-TOKEN>")

resp, _ := client.GetBlockByLimitNext2(ctx, &api.BlockLimit{StartNum: 68000000, EndNum: 68000005})
fmt.Println(resp)
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Message

`BlockListExtention`

| Field | Type                    | Description                   |
| ----- | ----------------------- | ----------------------------- |
| block | repeated BlockExtention | Blocks in the requested range |

## Use Cases

* **Batch Indexing**: Fetch a window of blocks in one request
* **Backfill**: Catch up a store over a range of heights
* **Analytics**: Read a contiguous span of blocks
* **Reorg Windows**: Re-scan a recent range after a rollback

## Error Handling

| Code | Status            | Description                                                 |
| ---- | ----------------- | ----------------------------------------------------------- |
| 3    | INVALID\_ARGUMENT | A request field is missing or malformed                     |
| 5    | NOT\_FOUND        | The requested account, block, or transaction does not exist |
| 16   | UNAUTHENTICATED   | The access token is missing or invalid                      |
| 13   | INTERNAL          | The node failed to process the request                      |
