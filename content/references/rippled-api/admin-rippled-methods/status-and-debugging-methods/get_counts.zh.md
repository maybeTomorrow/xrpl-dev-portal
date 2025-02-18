---
html: get_counts.html
parent: status-and-debugging-methods.html
blurb: Get statistics about the server's internals and memory usage.
---
# get_counts
[[Source]](https://github.com/ripple/rippled/blob/c7118a183a660648aa88a3546a6b2c5bce858440/src/ripple/rpc/handlers/GetCounts.cpp "Source")

The `get_counts` command provides various stats about the health of the server, mostly the number of objects of different types that it currently holds in memory.

_The `get_counts` method is an [admin method](admin-rippled-methods.html) that cannot be run by unprivileged users._

### Request Format
请求格式的示例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
    "id": 90,
    "command": "get_counts",
    "min_count": 100
}
```

*JSON-RPC*

```json
{
    "method": "get_counts",
    "params": [
        {
            "min_count": 100
        }
    ]
}
```

*Commandline*

```sh
#Syntax: get_counts [min_count]
rippled get_counts 100
```

<!-- MULTICODE_BLOCK_END -->

请求包括以下参数:

| `Field`     | Type                      | Description                        |
|:------------|:--------------------------|:-----------------------------------|
| `min_count` | Number (Unsigned Integer) | Only return fields with a value at least this high. |

### Response Format

成功响应的示例:

<!-- MULTICODE_BLOCK_START -->

*JSON-RPC*

```json
{
   "result" : {
      "AL_hit_rate" : 48.36725616455078,
      "HashRouterEntry" : 3048,
      "Ledger" : 46,
      "NodeObject" : 10417,
      "SLE_hit_rate" : 64.62035369873047,
      "STArray" : 1299,
      "STLedgerEntry" : 646,
      "STObject" : 6987,
      "STTx" : 4104,
      "STValidation" : 610,
      "Transaction" : 4069,
      "dbKBLedger" : 10733,
      "dbKBTotal" : 39069,
      "dbKBTransaction" : 26982,
      "fullbelow_size" : 0,
      "historical_perminute" : 0,
      "ledger_hit_rate" : 71.0565185546875,
      "node_hit_rate" : 3.808214902877808,
      "node_read_bytes" : 393611911,
      "node_reads_hit" : 1283098,
      "node_reads_total" : 679410,
      "node_writes" : 1744285,
      "node_written_bytes" : 794368909,
      "status" : "success",
      "treenode_cache_size" : 6650,
      "treenode_track_size" : 598631,
      "uptime" : "3 hours, 50 minutes, 27 seconds",
      "write_load" : 0
   }
}
```

*Commandline*

```json
Loading: "/etc/rippled.cfg"
Connecting to 127.0.0.1:5005

{
   "result" : {
      "AL_hit_rate" : 48.36725616455078,
      "HashRouterEntry" : 3048,
      "Ledger" : 46,
      "NodeObject" : 10417,
      "SLE_hit_rate" : 64.62035369873047,
      "STArray" : 1299,
      "STLedgerEntry" : 646,
      "STObject" : 6987,
      "STTx" : 4104,
      "STValidation" : 610,
      "Transaction" : 4069,
      "dbKBLedger" : 10733,
      "dbKBTotal" : 39069,
      "dbKBTransaction" : 26982,
      "fullbelow_size" : 0,
      "historical_perminute" : 0,
      "ledger_hit_rate" : 71.0565185546875,
      "node_hit_rate" : 3.808214902877808,
      "node_read_bytes" : 393611911,
      "node_reads_hit" : 1283098,
      "node_reads_total" : 679410,
      "node_writes" : 1744285,
      "node_written_bytes" : 794368909,
      "status" : "success",
      "treenode_cache_size" : 6650,
      "treenode_track_size" : 598631,
      "uptime" : "3 hours, 50 minutes, 27 seconds",
      "write_load" : 0
   }
}
```

<!-- MULTICODE_BLOCK_END -->

The response follows the [standard format][]. The list of fields contained in the result is subject to change without notice, but it may contain any of the following (among others):

| `Field`       | Type   | Description                                         |
|:--------------|:-------|:----------------------------------------------------|
| `Transaction` | Number | The number of `Transaction` objects in memory       |
| `Ledger`      | Number | The number of ledgers in memory                     |
| `uptime`      | String | The amount of time this server has been running uninterrupted. |

For most other entries, the value indicates the number of objects of that type currently in memory.

### Possible Errors

* 任何[通用错误类型][]。
* `invalidParams` - 一个或多个字段指定不正确，或者缺少一个或多个必需字段。

<!--{# common link defs #}-->
{% include '_snippets/rippled-api-links.md' %}
{% include '_snippets/tx-type-links.md' %}
{% include '_snippets/rippled_versions.md' %}
