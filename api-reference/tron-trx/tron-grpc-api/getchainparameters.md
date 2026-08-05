# GetChainParameters

This method returns the current network parameters set by on-chain governance, such as Energy and Bandwidth prices and transaction fees.

## Service

This method is served by the `protocol.Wallet` service.

## Method

```protobuf
rpc GetChainParameters (EmptyMessage) returns (ChainParameters)
```

## Request Message

`EmptyMessage`

This message has no fields.

## Request

{% tabs %}
{% tab title="grpcurl" %}
{% code overflow="wrap" %}
```bash
grpcurl -H 'x-api-key: <ACCESS-TOKEN>' \
  -d '{}' \
  go.getblock.io:443 protocol.Wallet/GetChainParameters
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
channel = grpc.secure_channel('go.getblock.io:443', creds)
metadata = [('x-api-key', '<ACCESS-TOKEN>')]
stub = api_pb2_grpc.WalletStub(channel)

response = stub.GetChainParameters(Tron_pb2.EmptyMessage(), metadata=metadata)
print(response)
```
{% endcode %}
{% endtab %}

{% tab title="Go" %}
{% code title="example.go" %}
```go
conn, _ := grpc.Dial(
    "go.getblock.io:443",
    grpc.WithTransportCredentials(credentials.NewTLS(&tls.Config{})),
)
defer conn.Close()

client := api.NewWalletClient(conn)
ctx := metadata.AppendToOutgoingContext(
    context.Background(), "x-api-key", "<ACCESS-TOKEN>")

resp, _ := client.GetChainParameters(ctx, &api.EmptyMessage{})
fmt.Println(resp)
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Message

`ChainParameters`

| Field                   | Type                    | Description                               |
| ----------------------- | ----------------------- | ----------------------------------------- |
| chainParameter          | repeated ChainParameter | Network parameters as key and value pairs |
| chainParameter\[].key   | string                  | Parameter name, such as getEnergyFee      |
| chainParameter\[].value | int64                   | Parameter value, in SUN where a price     |

## Use Cases

* **Fee Calculation**: Read the Energy price to compute call costs
* **Cost Modeling**: Read transaction and account-creation fees
* **Governance Tracking**: Detect parameter changes voted on-chain
* **Wallet Configuration**: Configure fee estimates from live parameters

## Error Handling

| Code | Status            | Description                                                 |
| ---- | ----------------- | ----------------------------------------------------------- |
| 3    | INVALID\_ARGUMENT | A request field is missing or malformed                     |
| 5    | NOT\_FOUND        | The requested account, block, or transaction does not exist |
| 16   | UNAUTHENTICATED   | The access token is missing or invalid                      |
| 13   | INTERNAL          | The node failed to process the request                      |
