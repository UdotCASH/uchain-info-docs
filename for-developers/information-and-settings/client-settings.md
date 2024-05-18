# Client Setting Requirements

## Supported Clients

uchaininfo currently supports Erigon, Nethermind, Geth, Parity, OpenEthereum, Hyperledger Besu, and Ganache clients. To define the node variant, it's advised to define the `ETHEREUM_JSONRPC_VARIANT` environment variable. Correct values include:

1. `parity` the same for Parity, OpenEthereum and Nethermind (default)
2. `erigon`
3. `geth`
4. `besu`
5. `ganache`

{% hint style="info" %}
uchaininfo currently requires a full archive node in order to import every state change for every address on the target network.
{% endhint %}

### Development

Explorer: [https://github.com/poanetwork/uchaininfo/blob/master/apps/explorer/config/dev.exs](https://github.com/poanetwork/uchaininfo/blob/master/apps/explorer/config/dev.exs)

Indexer: [https://github.com/poanetwork/uchaininfo/blob/master/apps/indexer/config/dev.exs](https://github.com/poanetwork/uchaininfo/blob/master/apps/indexer/config/dev.exs)

```
variant =
  if is_nil(System.get_env("ETHEREUM_JSONRPC_VARIANT")) do
    "ganache"
  else
    System.get_env("ETHEREUM_JSONRPC_VARIANT")
    |> String.split(".")
    |> List.last()
    |> String.downcase()
  end
```

### Production

Explorer: [https://github.com/poanetwork/uchaininfo/blob/master/apps/explorer/config/dev.exs](https://github.com/poanetwork/uchaininfo/blob/master/apps/explorer/config/dev.exs)

Indexer: [https://github.com/poanetwork/uchaininfo/blob/master/apps/indexer/config/prod.exs](https://github.com/poanetwork/uchaininfo/blob/master/apps/indexer/config/prod.exs)

```
variant =
  if is_nil(System.get_env("ETHEREUM_JSONRPC_VARIANT")) do
    "parity"
  else
    System.get_env("ETHEREUM_JSONRPC_VARIANT")
    |> String.split(".")
    |> List.last()
    |> String.downcase()
  end
```

## OpenEthereum Client

```
--jsonrpc-interface all --jsonrpc-apis web3,eth,net,parity,pubsub,traces --ws-interface all --fat-db=on --pruning=archive --ws-apis all --ws-origins all --ws-hosts all
```

| Name                    | Environment Variable         | Default Value                                  | Description                                                                                                                                      |
| ----------------------- | ---------------------------- | ---------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| **HTTP Endpoint**       | `ETHEREUM_JSONRPC_HTTP_URL`  | [http://localhost:8545](http://localhost:8545) | The HTTP Endpoint is used to fetch `blocks`, `transactions`, `receipts`, `coin/token balances`.                                                  |
| **Tracing Endpoint**    | `ETHEREUM_JSONRPC_TRACE_URL` | [http://localhost:8545](http://localhost:8545) | The Tracing endpoint is used to fetch `internal transactions` and `block traces`. In most cases this endpoint is identical to the HTTP Endpoint. |
| **WebSockets Endpoint** | `ETHEREUM_JSONRPC_WS_URL`    | ws://localhost:8546                            | The WebSockets endpoint subscribes to `newHeads` which alerts the indexer to fetch the new block from the subscription.                          |

### Development

Explorer: [https://github.com/poanetwork/uchaininfo/blob/59d8423e7ca3f608dbea411d4a0dc9bb4662a891/apps/explorer/config/dev/parity.exs#L8-L22](https://github.com/poanetwork/uchaininfo/blob/59d8423e7ca3f608dbea411d4a0dc9bb4662a891/apps/explorer/config/dev/parity.exs#L8-L22)

Indexer: [https://github.com/poanetwork/uchaininfo/blob/59d8423e7ca3f608dbea411d4a0dc9bb4662a891/apps/indexer/config/dev/parity.exs#L9-L23](https://github.com/poanetwork/uchaininfo/blob/59d8423e7ca3f608dbea411d4a0dc9bb4662a891/apps/indexer/config/dev/parity.exs#L9-L23)

### Production

Explorer: [https://github.com/poanetwork/uchaininfo/blob/59d8423e7ca3f608dbea411d4a0dc9bb4662a891/apps/explorer/config/prod/parity.exs#L8-L22](https://github.com/poanetwork/uchaininfo/blob/59d8423e7ca3f608dbea411d4a0dc9bb4662a891/apps/explorer/config/prod/parity.exs#L8-L22)

Indexer: [https://github.com/poanetwork/uchaininfo/blob/59d8423e7ca3f608dbea411d4a0dc9bb4662a891/apps/indexer/config/prod/parity.exs#L9-L23](https://github.com/poanetwork/uchaininfo/blob/59d8423e7ca3f608dbea411d4a0dc9bb4662a891/apps/indexer/config/prod/parity.exs#L9-L23)

## Geth Client

More information on Geth JSON-RPC [available here](https://geth.ethereum.org/docs/rpc/server).

```
sudo /usr/bin/geth --http --http.addr 0.0.0.0 --port 30303 --http.port 8545 --http.api debug,net,eth,shh,web3,txpool --ws.api "eth,net,web3,network,debug,txpool" --ws --ws.addr 0.0.0.0 --ws.port 8546 --ws.origins "*" --sepolia --datadir=/rinkeby --syncmode "full" --gcmode "archive" --http.vhosts "*"
```

_Tracing and pruning: By default, state for the last 128 blocks kept in memory. Most states are garbage collected. If you are running a block explorer or other service relying on transaction tracing without an archive node (--gcmode=archive), you need to trace within this window! Alternatively, specify the "reexec" tracer option to allow regenerating historical state; and ideally switch to chain tracing which amortizes overhead across all traced blocks._

| Name                    | Environment Variable        | Default Value                                  | Description                                                                                                             |
| ----------------------- | --------------------------- | ---------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| **HTTP Endpoint**       | `ETHEREUM_JSONRPC_HTTP_URL` | [http://localhost:8545](http://localhost:8545) | The HTTP Endpoint is used to fetch `blocks`, `transactions`, `receipts`, `coin/token balances`.                         |
| **WebSockets Endpoint** | `ETHEREUM_JSONRPC_WS_URL`   | ws://localhost:8546                            | The WebSockets endpoint subscribes to `newHeads` which alerts the indexer to fetch the new block from the subscription. |

### Development

Explorer: [https://github.com/poanetwork/uchaininfo/blob/59d8423e7ca3f608dbea411d4a0dc9bb4662a891/apps/explorer/config/dev/geth.exs#L8-L17](https://github.com/poanetwork/uchaininfo/blob/59d8423e7ca3f608dbea411d4a0dc9bb4662a891/apps/explorer/config/dev/geth.exs#L8-L17)

Indexer: [https://github.com/poanetwork/uchaininfo/blob/59d8423e7ca3f608dbea411d4a0dc9bb4662a891/apps/indexer/config/dev/geth.exs#L9-L18](https://github.com/poanetwork/uchaininfo/blob/59d8423e7ca3f608dbea411d4a0dc9bb4662a891/apps/indexer/config/dev/geth.exs#L9-L18)

### Production

Explorer: [https://github.com/poanetwork/uchaininfo/blob/59d8423e7ca3f608dbea411d4a0dc9bb4662a891/apps/explorer/config/prod/geth.exs#L8-L17](https://github.com/poanetwork/uchaininfo/blob/59d8423e7ca3f608dbea411d4a0dc9bb4662a891/apps/explorer/config/prod/geth.exs#L8-L17)

Indexer: [https://github.com/poanetwork/uchaininfo/blob/59d8423e7ca3f608dbea411d4a0dc9bb4662a891/apps/indexer/config/prod/geth.exs#L9-L18](https://github.com/poanetwork/uchaininfo/blob/59d8423e7ca3f608dbea411d4a0dc9bb4662a891/apps/indexer/config/prod/geth.exs#L9-L18)
