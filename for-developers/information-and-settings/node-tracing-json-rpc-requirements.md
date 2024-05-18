# Node Tracing / JSON RPC Requirements

### Standard JSON RPC Requirements

* Archive node with JSON RPC interface should strictly follow input/output interface described in [https://eth.wiki/json-rpc/API](https://eth.wiki/json-rpc/API).
* WebSocket interface (optional/highly recommended to enable) is used to subscribe to new block heads. Otherwise, UCHAIN.INFO will trigger new blocks fetching by periodical polling the JSON RPC `eth_blockNumber` method.
* Support for the following standard Ethereum JSON RPC methods:
  * [`eth_blockNumber`](https://eth.wiki/json-rpc/API#eth\_blocknumber)
  * [`eth_call`](https://eth.wiki/json-rpc/API#eth\_call)
  * [`eth_getBalance`](https://eth.wiki/json-rpc/API#eth\_getbalance)
  * [`eth_getCode`](https://eth.wiki/json-rpc/API#eth\_getcode)
  * [`eth_getBlockByHash`](https://eth.wiki/json-rpc/API#eth\_getblockbyhash)
  * [`eth_getBlockByNumber`](https://eth.wiki/json-rpc/API#eth\_getblockbynumber)
  * [`eth_getTransactionByHash`](https://eth.wiki/json-rpc/API#eth\_gettransactionbyhash)
  * [`eth_getTransactionByBlockHashAndIndex`](https://eth.wiki/json-rpc/API#eth\_gettransactionbyblockhashandindex)
  * [`eth_getTransactionByBlockNumberAndIndex`](https://eth.wiki/json-rpc/API#eth\_gettransactionbyblocknumberandindex)
  * [`eth_getTransactionReceipt`](https://eth.wiki/json-rpc/API#eth\_gettransactionreceipt)
  * [`eth_getUncleByBlockHashAndIndex`](https://eth.wiki/json-rpc/API#eth\_getunclebyblockhashandindex)
  * [`eth_getLogs`](https://eth.wiki/json-rpc/API#eth\_getlogs)

### **Fetching Pending Transactions**

| Client                                      | Method                                                                         |
| ------------------------------------------- | ------------------------------------------------------------------------------ |
| Erigon<br/>Nethermind<br/>OpenEthereum | <code>parity_pendingTransactions</code>                      |
| Geth                                        | <code>txpool_content</code> |

### Enable Tracing to Fetch Internal Transactions

| Client                                      | Method                                                                                                                                                                                                                                                                                                                                                                                                         |
| ------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Erigon<br/>Nethermind<br/>OpenEthereum | <ul><li><code>trace_replayBlockTransactions</code> (fetching of internal transactions)</li><li><code>trace_block</code> (fetching of block rewards)</li></ul>                                                                                                                                                                                                                                                                                            |
| Geth                                        | <ul><li><code>debug_traceBlockByNumber</code></li><li>or <code>debug_traceTransaction</code></li></ul> <code>callTracer</code> is used by default, starting from the UCHAIN.INFO 5.1.0 release. To switch to a custom JS tracer, the UCHAIN.INFO maintainer should set the <code>export INDEXER_INTERNAL_TRANSACTIONS_TRACER_TYPE=js</code> environment variable. Prior to the 5.1.0 release, js tracer was a default option. |

### JSON RPC Performance Benchmarks

Time measures for response time of crucial JSON RPC methods for indexing in UCHAIN.INFO. [Ways to improve speed](../../about/faqs.md#how-do-i-speed-up-my-self-hosted-instance).

1. `eth_getBlockByNumber` without transaction receipts for a block with 15 transactions: Desired response time is < 0.5s. For instance, in case of the Gnosis chain archive node, the response time for the block with \~20 transactions is \~0.4s.
2. `eth_getTransactionReceipt` for random transactions desired response time is < 0.5s. For the Gnosis chain archive node the response time is \~0.3 - 0.4s.
3. Batched `eth_getTransactionReceipt` for 15 transactions acceptable response time is Less than 1s. For the Gnosis chain archive node, it is \~0.6 - 0.7s

### Rate Limit

The desired rate limit for RPC endpoint is 200 req/sec for the indexing phase and 100 req/sec for the indexed chain.

## EVM Requirements & Clients

* All EVM chains must [define these variables](deployment-differences-between-chains.md) during configuration.
* uchaininfo currently supports Erigon, Geth, Nethermind, Hyperledger Besu, and Ganache clients. Define the node variant using the `ETHEREUM_JSONRPC_VARIANT` environment variable. [More information](client-settings.md)
